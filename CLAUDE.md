# EventoPedido

App de pedidos digitais para eventos e shows que elimina filas no bar. O consumidor escaneia um QR Code fixo do estabelecimento, faz o pedido pelo celular, paga via Pix ou cartão e retira no balcão apresentando um QR Code de comprovante. O operador do bar escaneia esse QR em um app nativo e confirma a entrega.

## Documentação (fonte da verdade)
Antes de implementar qualquer coisa ligada a requisitos, arquitetura, API, banco de dados ou interface, consulte:

- `docs/especificacao.md`: requisitos, fluxos (seção 5), segurança (6), arquitetura e autenticação (7), **rotas da API (7.5)** e **dicionário de dados com as 10 tabelas (8)**
- `docs/Documentação-Proj_01.docx`: versão original em Word do mesmo documento
- `docs/prototipos/prototipo-eventobeer-azul.html`: consumidor (seleção de evento → cardápio → Pix/cartão → comprovante, com avisos de pedido em aberto) e app do operador (login com código, scanner, atendimento, "Acabou", confirmar entrega e cancelar atendimento)
- `docs/prototipos/prototipo-organizador-azul.html`: painel do estabelecimento (login/cadastro, aprovação, conexão da conta Mercado Pago, dashboard, eventos, cardápios, itens do evento, operadores, link e QR Code, pedidos ao vivo)
- `docs/prototipos/prototipo-admin-azul.html`: painel do administrador (visão geral, faturamento mensal, busca de pedido, cadastros)

Não invente requisitos que não estejam documentados sem sinalizar a decisão. Se um pedido entrar em conflito com a documentação, os protótipos ou o modelo de dados, avise antes de implementar e explique o impacto. Quando uma decisão mudar o que está documentado, atualize `docs/especificacao.md` no mesmo PR.

## Estrutura do monorepo
```
backend/          API Node.js + TypeScript + Express + Prisma
apps/web/         Flutter Web: consumidor, estabelecimento e administrador
apps/operador/    Flutter nativo (Android e iOS) do operador
packages/core/    modelos, tema e utilitários compartilhados entre os apps Flutter
docs/             especificação e protótipos
```

## Stack
- **Backend**: Node.js 20 + TypeScript 5 + Express, hospedado no Railway
- **Banco + Auth + Realtime**: Supabase (PostgreSQL 15)
- **ORM**: Prisma (migrations e queries tipadas)
- **Validação**: Zod em todos os inputs de rota
- **Pagamentos**: Mercado Pago (Pix + cartão), com OAuth por estabelecimento e split pelo `application_fee`. Split: 5% plataforma; o estabelecimento recebe o restante na própria conta Mercado Pago, descontada a taxa do Mercado Pago. Chargebacks ficam com o estabelecimento
- **Flutter**: versão estável mais recente, Riverpod para estado
- **Gerenciador de pacotes Node**: pnpm

## Identidade visual
- Azul principal: #2B6CB0 | Azul escuro: #1A4F8A | Azul claro: #EBF4FF
- Azul médio: #63B3ED | Borda azul: #BEE3F8
- Fundo das telas: #F7FAFC | Cards e superfícies: #FFFFFF | Bordas: #E2E8F0
- Texto principal: #2D3748 | Texto secundário: #718096
- Sucesso: #38A169 | Alerta: #D69E2E | Erro: #E53E3E
- Fonte headings: Space Grotesk | Fonte interface: Inter
- Siga rigorosamente o layout dos protótipos em `docs/prototipos/`

## Arquitetura do backend
```
backend/src/
  routes/        definição de rotas e middlewares
  controllers/   recebe req/res, chama service, retorna resposta
  services/      regras de negócio
  repositories/  acesso ao banco (Prisma)
  middlewares/   auth, validação, error handler
  schemas/       schemas Zod
  types/         interfaces TypeScript
  utils/         funções auxiliares puras
  jobs/          rotinas agendadas (limpeza de sessões anônimas, expiração de pedidos Pix, renovação dos tokens do Mercado Pago)
```

## Autenticação por perfil
- **Estabelecimento e admin**: Supabase Auth com e-mail e senha. `establishments.user_id` aponta para `auth.users`. Admin identificado por `app_metadata.role = "admin"`
- **Operador**: não tem conta no Supabase Auth. O backend valida o código (hash HMAC-SHA-256 em `operators.access_code_hash`), emite um token próprio que expira em `events.ends_at` e confere `is_active` a cada requisição. Limite de 5 tentativas de login por IP, contadas em memória (ex.: `express-rate-limit`). Isso só funciona com a instância única da Railway, uma limitação conhecida: com várias instâncias, a contagem precisa ir para o banco ou para um cache compartilhado
- **Cliente**: sessão anônima do Supabase Auth, criada de forma invisível, com CAPTCHA invisível. Pedidos ficam ligados a `orders.client_id`. Rotina diária remove sessões sem pedido em aberto e inativas há mais de 24 h, esvaziando `client_id` antes

## Acesso a dados
- Toda gravação passa pelo backend (Prisma)
- O Flutter acessa o Supabase direto só para Auth e Realtime
- RLS ativo em todas as tabelas, com políticas apenas de leitura para o Realtime: o estabelecimento lê os pedidos dos próprios eventos; cada sessão anônima lê só os próprios pedidos
- Sessões anônimas usam o papel `authenticated`: toda política confere a posse por `auth.uid()`, nunca só o fato de estar autenticado
- Data API do Supabase desligada
- `auth.users` não é gerenciada pelo Prisma: a FK para ela é criada em migration SQL manual

## Regras de negócio críticas
- Status do evento é calculado (rascunho, agendado, ativo, encerrado) a partir de `published_at`, `starts_at`, `ends_at`; não existe coluna de status
- Pedido: `awaiting_payment → paid → in_service → collected`; também `in_service → paid` (cancelar atendimento), `awaiting_payment → expired` (15 min) e `expired → paid` (pagamento atrasado com o evento em andamento). Não existe `refunded`
- A reserva na leitura é atômica (`UPDATE ... WHERE status = 'paid'`). Um operador só tem um atendimento aberto por vez, verificado no backend
- Validade do QR = `status = 'paid'` e evento não encerrado. Não existe `is_qr_valid`
- "Acabou" no app do operador: na confirmação da entrega, os itens marcados ficam com `order_items.is_undelivered = true` e o item do evento fica `is_available = false`. O item esgotado continua no cardápio, acinzentado e com o selo "Esgotado", sem botão de adicionar. Troca ou devolução é combinada fora da plataforma
- Cardápios do estabelecimento (`menus`) são modelos: os itens são copiados para `menu_items` na criação do evento
- Estabelecimento com `approval_status` diferente de `approved` não publica eventos e não recebe o link/QR. Publicar também exige a conta Mercado Pago conectada. `slug` não pode ser alterado pelo estabelecimento após a aprovação
- `order_code` no formato `GH7-382` (2 letras, 1 dígito, hífen, 3 dígitos), sem 0/O e 1/I, gerado aleatoriamente; se colidir, gera outro. É único só dentro do evento, então buscas globais (admin) podem retornar mais de um pedido

## Pagamentos
- Nunca considerar um pedido como pago com base apenas em informação enviada pelo cliente
- Cada estabelecimento conecta a própria conta Mercado Pago por OAuth; o pagamento é criado com o token do estabelecimento e a comissão no `application_fee` (5% do total, calculado no backend)
- Tokens do Mercado Pago (`mp_access_token`, `mp_refresh_token`) guardados criptografados (`TOKEN_ENCRYPTION_KEY`), nunca enviados ao app; job renova o token antes de `mp_token_expires_at`, salvando o novo refresh token de forma atômica
- A confirmação é validada pelo backend: webhook em `/webhooks/mercadopago` com verificação de assinatura e nova consulta do pagamento na API, registrando cada aviso em `payment_webhooks`
- Operações de pagamento devem ser idempotentes
- Uma tentativa pendente por pedido: cancelar o Pix pendente no Mercado Pago antes de criar outra tentativa
- Pix criado no Mercado Pago com validade de 15 min; pagamento aprovado que chega para pedido `expired` reativa o pedido como `paid` se o evento ainda estiver em andamento
- Nunca armazenar dados de cartão; tokenizar no Flutter com o Mercado Pago
- Não armazenar dados bancários do estabelecimento
- Registrar todos os estados e transições de pagamento em `payments`
- Sem estornos pela plataforma no MVP
- Valor do pedido sempre calculado no backend a partir de `menu_items`
- Pagamento em duplicidade (segunda tentativa aprovada para pedido já pago): fica registrado em `payments`, aparece na busca do admin e o estabelecimento devolve pelo próprio Mercado Pago

## Autorização
- Autenticação e autorização tratadas separadamente
- Validar se o estabelecimento/evento/operador pertence ao usuário antes de qualquer operação
- Nunca confiar em IDs enviados pelo cliente para determinar permissões

## Boas práticas
- TypeScript strict mode; evite `any`: use tipagem correta ou `unknown` com type guard
- Funções com responsabilidade única; refatore o que ficar longo ou complexo
- Nomes de variáveis e funções em inglês; comentários em português
- Erros controlados com a classe `AppError` e middleware global de error handler
- Nunca colocar credenciais no código: usar `.env` (com `.env.example` no repositório). Nunca commitar `.env`
- Migrations para toda alteração de schema, nunca editar direto no painel do Supabase

## Flutter
- Riverpod para estado
- Separação: screens/ → widgets/ → services/ → models/
- Lógica de negócio fora dos widgets
- Tratar os estados de loading, erro e vazio em todas as telas
- O ID do app do operador é provisório (nome do produto ainda não definido); manter o `TODO` que marca onde trocar antes da publicação

## Testes
- Testar regras de negócio críticas e os endpoints principais
- Fluxos obrigatórios: webhook repetido, leitura simultânea do mesmo QR, item que acabou na confirmação da entrega, expiração do Pix, Pix pago após expirar (reativa o pedido), pagamento em duplicidade, renovação do token do Mercado Pago, acesso a recurso de outro estabelecimento
- Uma tarefa só está concluída quando os testes necessários estiverem passando

## Fluxo de trabalho
1. Antes de começar uma issue, crie a branch a partir da `main` atualizada: `<tipo>/<numero>-<descricao-curta>` (tipos: `feat`, `fix`, `chore`, `docs`, `refactor`, `test`). Nunca trabalhe na `main`.
2. Faça o trabalho. Nunca faça commit nem push: isso é feito pelo Vinicius.
3. Ao terminar, confira: testes, lint e checagem de tipos passando, nenhum `.env` ou chave nos arquivos. Depois mostre um resumo do que mudou e sugira o título do commit no padrão convencional, em português.
4. Depois que o Vinicius fizer o commit e o Sync Changes, abra o PR com `gh pr create`, com título igual ao do commit e descrição com: o que foi feito, como testar e `Closes #<numero>`.
5. Faça o merge com `gh pr merge --merge`. Não apague a branch.
6. Volte para a `main` e atualize (`git switch main` e `git pull`).

## Comportamento
- Responda sempre em português
- Se houver abordagens diferentes para um problema, apresente as opções brevemente antes de escolher
- Se perceber inconsistência nos requisitos ou protótipos, aponte antes de implementar
