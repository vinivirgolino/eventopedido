# EventoPedido — Especificação do projeto

> Versão em Markdown do Word `Documentação-Proj_01.docx`, para leitura pelo Claude Code. Em caso de divergência, atualize os dois.

# 1 INTRODUÇÃO

A experiência do consumidor em eventos é frequentemente comprometida por longas filas nos pontos de venda de bebidas e alimentos. Esse gargalo operacional impacta dois lados ao mesmo tempo: o consumidor, que precisa se ausentar do evento por longos períodos para realizar uma compra simples, e o estabelecimento, que perde receita quando parte do público desiste da compra diante da espera. Em eventos com grande fluxo de pessoas, esse problema se intensifica, gerando insatisfação generalizada e redução do potencial de faturamento.

São Paulo consolidou-se como o maior polo de eventos da América Latina. Segundo a São Paulo Convention & Visitors Bureau, mais de 3.900 eventos foram realizados na capital paulista apenas no primeiro semestre de 2025, representando crescimento de quase 60% em relação ao mesmo período de 2024, com impacto econômico de R\$ 12,8 bilhões. No contexto nacional, a ABRAPE projeta R\$ 141,1 bilhões gerados pelo setor de eventos em 2025, com mais de 7 milhões de empregos diretos e indiretos. Segundo o site BaresSP, em São Paulo acontece em média pelo menos um evento a cada seis minutos, evidenciando a dimensão e o dinamismo desse mercado.

Nesse cenário de crescimento acelerado, a ausência de soluções tecnológicas acessíveis para o gerenciamento de pedidos dentro dos eventos representa uma oportunidade concreta de mercado. A maioria dos estabelecimentos ainda opera com processos manuais — fichas físicas, comandas em papel e caixas convencionais — que não acompanharam a evolução digital do setor. Este documento registra o processo de concepção de uma plataforma que endereça esse problema de forma direta, desde a identificação da dor até a definição da arquitetura técnica e do modelo de negócio.

## 1.1 Objetivos

### **1.1.1 Objetivo Geral**

Desenvolver uma plataforma digital que permita a consumidores realizarem pedidos e pagamentos em eventos diretamente pelo celular, eliminando a necessidade de enfrentar filas no caixa e simplificando a operação dos estabelecimentos.

### **1.1.2 Objetivos Específicos**

- Mapear o fluxo de compra atual em eventos e identificar os principais pontos de fricção e gargalos operacionais.

- Definir os perfis de usuário da plataforma e especificar os fluxos de interação de cada um com o sistema.

- Especificar a arquitetura técnica que suporte o desenvolvimento, a segurança e a escalabilidade da solução.

- Estruturar o dicionário de dados do sistema, definindo as tabelas, campos, tipos e relacionamentos do banco de dados.

- Definir um modelo de negócio sustentável, com baixo custo operacional e receita automatizada.

- Elaborar protótipos interativos das interfaces do cliente, do operador de bar e do estabelecimento.

## 1.2 Justificativa

O mercado de eventos brasileiro vive um momento de expansão sem precedentes, impulsionado pela retomada pós-pandemia e pelo crescimento da economia criativa. Apesar desse crescimento expressivo, a operação de venda de consumação dentro dos eventos permanece majoritariamente dependente de processos manuais e analógicos, que geram gargalos operacionais, perda de receita e insatisfação do público.

A digitalização da experiência do consumidor dentro dos eventos é uma tendência natural e irreversível. A plataforma proposta neste documento posiciona-se para capturar essa oportunidade, oferecendo uma solução simples, segura e de fácil adoção para estabelecimentos de diferentes portes e segmentos — de casas de show e bares a festas universitárias, feiras e eventos corporativos.

# 2 IDENTIFICAÇÃO DO PROBLEMA

O modelo atual de venda de bebidas e alimentos em eventos funciona de forma linear e analógica: o consumidor identifica o que deseja consumir, desloca-se até o ponto de venda, aguarda em fila para ser atendido no caixa, realiza o pagamento em dinheiro ou cartão, recebe um comprovante físico ou uma ficha e, em seguida, aguarda em uma segunda fila no balcão para finalmente retirar o produto. Em eventos com grande fluxo de pessoas, esse processo pode resultar em esperas de 15 a 30 minutos ou mais, o que é incompatível com a dinâmica de um evento ao vivo.

Esse modelo gera prejuízos concretos para ambos os lados. Para o consumidor, representa tempo desperdiçado longe do evento que pagou para assistir, além de frustração e desconforto. Para o estabelecimento, representa perda direta de receita, pois parte significativa do público desiste da compra ao visualizar filas longas, optando por não consumir. Além disso, a gestão financeira em tempo real é praticamente impossível com processos manuais, dificultando decisões operacionais durante o próprio evento.

Os principais problemas identificados são:

- **Dupla espera:** o consumidor enfrenta fila tanto no caixa para pagar quanto no balcão para retirar o produto, dobrando o tempo total de espera.

- **Perda de receita:** consumidores desistem da compra ao visualizar filas longas, representando perda direta de faturamento para o estabelecimento.

- **Ausência de controle financeiro em tempo real:** o modelo manual não permite que o estabelecimento acompanhe o faturamento, os itens mais vendidos ou os gargalos operacionais durante o evento.

- **Custo recorrente de materiais:** soluções existentes geram um QR Code por evento, obrigando o estabelecimento a reimprimir banners, totens e pulseiras a cada novo evento, gerando custos recorrentes desnecessários.

- **Vulnerabilidade a fraudes:** fichas físicas e comprovantes em papel são facilmente falsificados ou reutilizados, gerando prejuízo operacional para o estabelecimento.

# 3 SOLUÇÃO PROPOSTA

A solução consiste em uma plataforma digital que elimina a fila no caixa ao permitir que o consumidor realize todo o processo de compra pelo próprio celular, dentro do evento, sem instalar nenhum aplicativo. O ponto de entrada é um QR Code fixo do estabelecimento — impresso em banners, totens e pulseiras — que o consumidor escaneia uma única vez para acessar a plataforma.

Ao escanear o QR Code, o consumidor é direcionado para uma tela que exibe os eventos do estabelecimento que estão acontecendo naquele momento e os que começam em breve. Com um único toque no evento desejado, acessa o cardápio, que exibe o nome do evento no topo durante toda a compra. A partir daí, escolhe os itens, informa as quantidades e realiza o pagamento via Pix ou cartão de crédito. Assim que o Pagar.me confirma o pagamento, a tela exibe o comprovante com um QR Code único gerado para aquele pedido. Para retirar o produto, basta se dirigir ao balcão e apresentar esse QR Code ao operador.

Do lado do operador, a operação é igualmente simples. Ele acessa um aplicativo nativo instalado no celular do bar, abre a câmera pelo próprio app, escaneia o QR Code do consumidor e o pedido aparece instantaneamente na tela com os itens e quantidades. No momento da leitura, o pedido fica reservado para aquele operador, o que impede que o mesmo QR Code seja atendido em outro balcão. O operador entrega os itens e confirma a entrega no aplicativo; só então o QR Code é invalidado definitivamente e o pedido é encerrado.

## 3.1 Inovações da Solução

A plataforma apresenta características que a diferenciam de soluções convencionais existentes no mercado:

- **QR Code fixo por estabelecimento:** o QR Code não muda a cada evento. Uma vez impresso em banners, totens e pulseiras, serve para todos os eventos futuros daquele estabelecimento, eliminando o custo recorrente de reimpressão de materiais gráficos.

- **Tela de seleção de evento: ao escanear o QR Code, o consumidor sempre visualiza uma tela com os eventos do estabelecimento que estão acontecendo naquele momento e escolhe o evento com um único toque. O nome do evento permanece visível no topo do cardápio durante toda a compra. Isso elimina o risco de o sistema direcioná-lo para o evento errado e gera confiança no processo de compra.**

- **Acesso sem instalação:** o consumidor acessa a plataforma diretamente pelo navegador do celular, via QR Code ou link compartilhado pelo estabelecimento. Não é necessário instalar nenhum aplicativo, criar uma conta ou fazer login.

- **Fluxo simplificado:** o processo de compra do consumidor resume-se a quatro ações objetivas — escanear o QR Code, escolher os itens, pagar e retirar o pedido no balcão.

- **Segurança antifraude por QR Code de uso único: cada pedido gera um QR Code único, vinculado exclusivamente àquele pedido. Na leitura, o pedido fica reservado para o operador que o escaneou, e o QR Code é invalidado definitivamente quando a entrega é confirmada, impossibilitando qualquer reutilização, inclusive por meio de capturas de tela.**

- **Repasse financeiro automático:** o gateway de pagamento realiza o split automático a cada transação, distribuindo os valores entre a plataforma e o estabelecimento sem necessidade de nenhuma ação manual.

# 4 PERFIS DE USUÁRIO

A plataforma foi projetada para atender quatro perfis de usuário distintos, cada um com responsabilidades, permissões e formas de acesso específicas. A separação clara entre os perfis garante que cada usuário acesse apenas o que é relevante para sua função, aumentando a segurança e simplificando a experiência de uso.

## 4.1 Administrador

O administrador é o próprio desenvolvedor e gestor da plataforma. Possui acesso irrestrito a todos os dados e funcionalidades do sistema. É responsável por aprovar o cadastro de novos estabelecimentos, monitorar a saúde operacional da plataforma, gerenciar a infraestrutura técnica e resolver eventuais inconsistências nos dados. O administrador não interage com os eventos diretamente, mas tem visibilidade total sobre todos os estabelecimentos, eventos, pedidos e operadores cadastrados na plataforma. O perfil é identificado por uma marcação no próprio usuário do Supabase Auth, que só pode ser alterada pelo servidor. O acesso ocorre por um painel web com quatro áreas: visão geral do dia, com os eventos acontecendo e o movimento da plataforma; faturamento mensal, com a quantidade de vendas, o valor transacionado, os estornos e a receita da plataforma, no total e por estabelecimento; busca de pedido pelo código, com os itens, as tentativas de pagamento, os estornos e o histórico de atendimento, para dar suporte aos estabelecimentos; e análise de cadastros, com as ações de aprovar ou rejeitar.

## 4.2 Estabelecimento

O estabelecimento representa a casa de show, o bar, o produtor cultural ou qualquer gestor de espaço de entretenimento que utiliza a plataforma para gerenciar a venda de consumação em seus eventos. Esse perfil concentra todas as responsabilidades de configuração e acompanhamento: cadastrar e publicar eventos, criar cardápios reutilizáveis com itens, preços e categorias, gerenciar os operadores de bar e acompanhar em tempo real os pedidos realizados e o faturamento acumulado. O cadastro exige CPF ou CNPJ, o que permite que produtores culturais pessoa física também utilizem a plataforma, e o acesso ao painel web é realizado com e-mail e senha.

## 4.3 Operador de Bar

O operador de bar é o funcionário responsável por um balcão de atendimento durante o evento. Seu papel na plataforma é exclusivamente operacional: atender os pedidos, confirmar a entrega e, quando um item acabar, registrar o estorno correspondente. Para acessar o sistema, o operador utiliza um aplicativo nativo instalado no celular do bar e digita um código de acesso individual de seis caracteres, gerado pelo estabelecimento especificamente para aquele evento. Cada operador possui seu próprio código, o que permite ao estabelecimento rastrear individualmente a atuação de cada funcionário e revogar acessos de forma pontual a qualquer momento. O cadastro do operador pode incluir o nome do balcão em que ele atua, utilizado apenas como informação no painel e nas mensagens do aplicativo. O código expira automaticamente ao término do evento, eliminando a necessidade de gerenciamento manual de acessos após o fim da operação.

## 4.4 Cliente

O cliente é o consumidor final presente no evento. Não precisa criar uma conta, fazer login ou instalar nenhum aplicativo para utilizar a plataforma. O consumidor escaneia o QR Code fixo do estabelecimento com a câmera do celular, é direcionado para a tela de seleção de eventos, escolhe o evento ativo, navega pelo cardápio, seleciona os itens desejados, realiza o pagamento e recebe o comprovante com o QR Code do pedido. Nos bastidores, a plataforma cria uma sessão anônima invisível no navegador, sem nenhuma tela ou dado pessoal, que permite reconhecer os pedidos em aberto daquele celular. Caso saia da tela do comprovante, ao voltar ao cardápio o consumidor encontra um aviso no topo da tela que o leva de volta ao pedido.

## 4.5 Dados de Acesso por Perfil

| **Perfil**      | **Dados de login**                                      | **Tipo de acesso**          |
|-----------------|---------------------------------------------------------|-----------------------------|
| Administrador   | E-mail e senha                                          | Painel web                  |
| Estabelecimento | E-mail e senha (cadastro com CPF ou CNPJ)               | Painel web                  |
| Operador de bar | Código de acesso individual de 6 caracteres, por evento | App nativo Flutter          |
| Cliente         | Sem login — sessão anônima invisível                    | Web app via QR Code ou link |

# 5 FLUXOS DE USO

Os fluxos de uso descrevem, de forma sequencial, todas as ações que cada perfil de usuário realiza na plataforma. A definição clara desses fluxos é fundamental para garantir que o sistema seja construído de forma coerente, sem lacunas operacionais ou contradições entre os perfis.

## 5.1 Fluxo do Cliente

O fluxo do cliente foi desenhado para ser o mais direto e intuitivo possível, eliminando qualquer etapa que não seja estritamente necessária para a realização da compra:

- Escaneia o QR Code fixo do estabelecimento com a câmera do celular.

- É direcionado automaticamente para a tela de seleção de eventos, que exibe os eventos acontecendo naquele momento e os que começam em breve. Eventos que ainda não começaram aparecem com o horário de início, por exemplo "Começa às 21h", sem permitir pedidos.

- Toca no evento desejado; o toque já confirma a escolha e abre o cardápio.

- Visualiza o cardápio do evento, organizado por categorias, com nome, emoji e preço de cada item. Itens esgotados aparecem acinzentados, com o selo "Esgotado", e não podem ser adicionados.

- Seleciona os itens desejados e define as quantidades.

- Avança para a tela de pagamento, onde confirma o resumo do pedido e escolhe a forma de pagamento: Pix ou cartão de crédito.

- No Pix, recebe o QR Code e o código copia e cola, com prazo de 15 minutos para pagar; a tela aguarda a confirmação e se atualiza sozinha. No cartão, preenche os dados em um formulário seguro; se o cartão for recusado, pode tentar novamente ou trocar para Pix sem refazer o pedido.

- Com o pagamento confirmado, recebe na tela o comprovante do pedido, contendo o QR Code único, a lista de itens, o valor total, o horário da compra e o horário limite para retirada, que é o fim do evento.

- Dirige-se a qualquer balcão do estabelecimento e apresenta o QR Code ao operador para retirar o pedido. O comprovante acompanha o status em tempo real: aguardando retirada, em atendimento e retirado.

- Caso saia da tela, ao voltar ao cardápio encontra um aviso no topo: "Pagamento pendente", quando o Pix ainda não foi confirmado; "Pedido disponível para retirada", quando já pagou e não retirou; ou "Pix expirado — nenhum valor foi cobrado", exibido uma única vez quando o prazo do Pix termina sem pagamento.

## 5.2 Fluxo do Operador de Bar

O operador de bar interage com a plataforma exclusivamente durante o evento, por meio do aplicativo nativo instalado no celular do balcão:

- Recebe o código de acesso individual enviado pelo estabelecimento antes do início do evento, normalmente por WhatsApp.

- Abre o aplicativo nativo no celular do bar e insere o código de seis caracteres. Após cinco tentativas incorretas, o dispositivo precisa aguardar alguns minutos antes de tentar novamente.

- Já autenticado, acessa a tela principal do app, que exibe o botão de escaneamento de pedidos e a lista de pedidos entregues por ele.

- Quando um consumidor se aproxima do balcão, pressiona o botão "Escanear pedido", que abre a câmera do dispositivo dentro do próprio aplicativo, e aponta a câmera para o QR Code exibido na tela do consumidor.

- O pedido é exibido instantaneamente, com a lista completa de itens e quantidades, e fica reservado para aquele operador. Enquanto o atendimento não for concluído, o aplicativo não permite escanear outro pedido.

- Se o QR Code não puder ser atendido, o aplicativo informa o motivo: pedido já retirado, pedido em atendimento em outro balcão, pagamento não confirmado ou evento encerrado.

- Entrega os itens ao consumidor e pressiona "Confirmar entrega". O pedido é encerrado, o QR Code é invalidado e o aplicativo libera uma nova leitura.

- Se algum item tiver acabado, marca o item como esgotado: o valor correspondente é estornado ao consumidor, o item deixa de ser vendido no cardápio do evento e o operador entrega o restante do pedido. Se o consumidor não quiser mais nada, o operador pode estornar o pedido inteiro.

- Se o atendimento precisar ser interrompido sem falta de produto, por exemplo porque o consumidor se afastou ou o QR Code foi lido por engano, pressiona "Cancelar atendimento": o pedido volta a ficar disponível para retirada, sem estorno.

- Se o aplicativo for fechado ou o celular travar durante um atendimento, ao reabrir o app o operador é levado de volta ao pedido que estava em aberto.

## 5.3 Fluxo do Estabelecimento

O estabelecimento interage com a plataforma principalmente na fase de configuração dos eventos e no acompanhamento da operação em tempo real:

- Realiza o cadastro na plataforma informando nome, e-mail, senha, CPF ou CNPJ, o identificador do link público (por exemplo, bar-do-ze) e os dados bancários. Os dados bancários são enviados diretamente ao Pagar.me para criar a conta de recebedor e não ficam armazenados na plataforma.

- Enquanto aguarda a aprovação do administrador, já pode acessar o painel e preparar cardápios, eventos e operadores, mas não pode publicar eventos.

- Após a aprovação, recebe o link e o QR Code fixo do estabelecimento, que serão utilizados em todos os eventos futuros sem necessidade de substituição. A partir desse momento, o identificador do link não pode mais ser alterado pelo estabelecimento.

- Cria um ou mais cardápios reutilizáveis, com nome, preço, categoria e emoji representativo de cada item. Os cardápios podem ser alterados ou excluídos a qualquer momento.

- Para cada novo evento, acessa o painel e cria um registro informando o nome do evento, a data e hora de início, a data e hora de término e o cardápio que será utilizado. Os itens do cardápio escolhido são copiados para o evento e podem ser ajustados apenas para aquela ocasião, sem alterar o cardápio original.

- Cadastra os operadores de bar que atuarão no evento, informando o nome de cada um e, opcionalmente, o balcão. A plataforma gera automaticamente um código de acesso individual, exibido uma única vez, com um botão para enviá-lo pelo WhatsApp com a mensagem já pronta. Se o operador perder o código, o estabelecimento gera um novo, e o anterior deixa de valer.

- Publica o evento. A partir desse momento, ele passa a aparecer na tela de seleção de eventos quando um consumidor escaneia o QR Code do estabelecimento: como "Começa às 21h" antes do início e com o cardápio liberado durante o evento.

- Durante o evento, acompanha em tempo real pelo painel os pedidos realizados, o faturamento acumulado, os itens mais vendidos e o status de cada pedido, e pode marcar itens como esgotados ou disponíveis novamente.

- Após o evento, consulta os estornos realizados, com o operador responsável, os itens e o motivo de cada um.

## 5.4 Canais de Acesso do Consumidor

A plataforma utiliza três canais complementares para garantir que o consumidor consiga acessar o cardápio sem nenhuma fricção, independentemente de como tomou conhecimento da solução:

| **Canal**                                   | **Quando alcança o consumidor**                                     | **Custo para o estabelecimento**              |
|---------------------------------------------|---------------------------------------------------------------------|-----------------------------------------------|
| QR Code fixo impresso na pulseira do evento | No momento da entrada — alcança todo consumidor que acessa o evento | Único — não precisa ser refeito a cada evento |
| QR Code fixo em banners e totens internos   | Durante o evento, próximo aos balcões de atendimento                | Único — não precisa ser refeito a cada evento |
| Link único divulgado pelo estabelecimento   | Antes do evento, via redes sociais, WhatsApp ou e-mail              | Zero                                          |

Quando o consumidor acessa o link antes do início de um evento publicado, o evento aparece na tela de seleção com o horário de início, por exemplo "Começa às 21h", sem permitir pedidos. O cardápio é liberado automaticamente no horário de início.

# 6 SEGURANÇA E ANTIFRAUDE

A segurança do sistema foi pensada desde o início do projeto, considerando que a plataforma lida com transações financeiras reais e com a confiança do consumidor no processo de compra. A principal vulnerabilidade identificada é a possibilidade de um consumidor tentar apresentar o mesmo comprovante mais de uma vez para retirar o pedido duplicadamente. Para eliminar esse risco, a plataforma implementa um conjunto de camadas de proteção que funcionam de forma complementar:

- QR Code de uso único: cada pedido pago gera um QR Code vinculado exclusivamente ao identificador único daquele pedido no banco de dados. A validade do QR Code não é armazenada em um campo próprio: ela é calculada no momento da leitura. O QR Code só é aceito se o pedido estiver com o status paid e o evento ainda não tiver terminado.

- Reserva atômica na leitura: ao escanear o QR Code, o pedido passa de paid para in_service e fica vinculado ao operador que o leu. O banco de dados só aceita essa mudança se o pedido ainda estiver com o status paid, de modo que duas leituras simultâneas nunca são aceitas ao mesmo tempo. Se um amigo apresentar a captura de tela do mesmo QR Code em outro balcão, o operador vê a mensagem "Pedido em atendimento no Bar Principal".

- Invalidação na confirmação da entrega: o QR Code é invalidado definitivamente quando o operador confirma a entrega, e o pedido passa para o status collected. Se o atendimento for cancelado sem entrega, o pedido volta para paid e o consumidor continua podendo retirá-lo.

- Um atendimento por vez: enquanto o operador não concluir ou cancelar o atendimento em andamento, o backend não aceita uma nova leitura daquele operador. A regra é verificada no servidor, e não apenas no aplicativo, e o atendimento em aberto é retomado automaticamente se o aplicativo for reiniciado.

- Validade temporal: além da invalidação por uso, o QR Code expira automaticamente ao término do evento, definido pelo campo ends_at da tabela events. Isso impede que um comprovante não utilizado seja apresentado em um evento futuro do mesmo estabelecimento.

- Feedback visual ao consumidor: o comprovante na tela do consumidor é atualizado em tempo real para exibir o status do pedido: aguardando retirada, em atendimento e retirado. A atualização é entregue apenas ao celular que fez o pedido, graças à sessão anônima e às políticas de segurança por linha do banco de dados.

- Estorno rastreável: quando um item acaba, o operador registra o estorno no próprio aplicativo. O valor sempre retorna ao meio de pagamento original do consumidor, nunca ao operador, e cada estorno é registrado com o identificador do operador, os itens, o valor e o motivo, disponível para auditoria no painel do estabelecimento.

- Rastreabilidade por operador: cada atendimento é registrado com o identificador do operador que realizou a leitura, por meio do campo operator_id na tabela orders, junto com os horários de leitura e de entrega. Isso permite auditoria completa de todas as entregas realizadas durante o evento.

- Código de acesso do operador protegido: o código possui seis caracteres alfanuméricos, sem caracteres que se confundem, como 0 e O, o que resulta em cerca de um bilhão de combinações. O backend limita as tentativas de login e armazena apenas o hash do código, nunca o código em texto puro.

- QR Code do comprovante válido em todos os balcões: o QR Code gerado no comprovante do consumidor é reconhecido por qualquer operador do mesmo evento, independentemente do balcão em que está alocado.

# 7 ARQUITETURA TÉCNICA

A arquitetura técnica da plataforma foi definida com dois critérios principais: custo zero na fase de desenvolvimento e ausência de retrabalho no futuro. Para atender ao primeiro critério, o banco de dados é hospedado no Supabase e o backend Node.js é hospedado na Railway. Para atender ao segundo, a escolha por banco de dados relacional desde o início elimina a necessidade de reestruturar os dados quando a plataforma escalar, evitando o custo e o risco de uma migração futura.

## 7.1 Stack Tecnológica

A stack foi selecionada com base em três critérios: maturidade e adoção no mercado, familiaridade do desenvolvedor com as tecnologias e coerência entre as camadas da aplicação. Todas as escolhas seguem padrões amplamente utilizados em produtos digitais profissionais:

| **Camada**                | **Tecnologia**                 | **Função**                                                                                       |
|---------------------------|--------------------------------|--------------------------------------------------------------------------------------------------|
| Frontend cliente          | Flutter Web                    | Tela de seleção de evento, cardápio, pagamento e comprovante                                     |
| Frontend estabelecimento  | Flutter Web                    | Painel de gerenciamento de eventos, cardápios, operadores e pedidos                              |
| Frontend administrador    | Flutter Web                    | Visão geral, faturamento mensal, busca de pedidos e aprovação de cadastros                       |
| App operador              | Flutter (nativo Android e iOS) | Scanner de QR Code, atendimento, confirmação de entrega e estorno de itens                       |
| Backend                   | Node.js + TypeScript + Express | Regras de negócio, validação de pedidos, autenticação dos operadores e integração com o Pagar.me |
| Acesso a dados            | Prisma                         | Consultas tipadas e migrations do banco de dados                                                 |
| Validação                 | Zod                            | Validação dos dados de entrada de todas as rotas da API                                          |
| Banco de dados            | Supabase (PostgreSQL)          | Banco de dados relacional, armazenamento, consultas e relatórios                                 |
| Hosting                   | Railway                        | Hospedagem do backend Node.js                                                                    |
| Autenticação              | Supabase Auth                  | Autenticação do estabelecimento e do administrador e sessões anônimas dos consumidores           |
| Comunicação em tempo real | Supabase Realtime              | Pedidos ao vivo no painel do estabelecimento e atualização do comprovante do consumidor          |
| Pagamento                 | Pagar.me                       | Processamento de pagamentos, split automático e estornos                                         |
| QR Code (geração)         | qr_flutter                     | Geração do QR Code no comprovante do consumidor                                                  |
| QR Code (leitura)         | mobile_scanner                 | Leitura do QR Code pelo app do operador                                                          |

## 7.2 Fases da Infraestrutura

A infraestrutura foi planejada em duas fases, permitindo que o desenvolvimento comece sem nenhum custo e que a migração para uma infraestrutura paga ocorra apenas quando houver faturamento real para sustentá-la:

| **Fase**                    | **Infraestrutura**                                                                     | **Custo mensal**      |
|-----------------------------|----------------------------------------------------------------------------------------|-----------------------|
| Desenvolvimento e validação | Supabase (banco de dados) + Railway (backend Node.js) — planos gratuitos               | Zero                  |
| Escala com clientes reais   | Supabase (banco de dados) + Railway (backend Node.js), com expansão conforme a demanda | A partir de USD 5/mês |

Na fase de escala, deve ser acompanhado o número de usuários ativos mensais do Supabase Auth, que inclui as sessões anônimas criadas para os consumidores. Esse número cresce com o público dos eventos e é um dos limites que determinam o plano contratado.

## 7.3 Autenticação por Perfil

A autenticação combina o Supabase Auth, responsável pelo gerenciamento de usuários e pela emissão de tokens JWT, com a validação feita pelo backend em Node.js, que confere o token em cada requisição e verifica se o recurso solicitado pertence ao usuário autenticado. Autenticação e autorização são tratadas separadamente, e o backend nunca utiliza identificadores enviados pelo cliente para decidir permissões. Cada perfil possui um fluxo de autenticação específico:

- Administrador: realiza o login com e-mail e senha pelo Supabase Auth. O perfil é identificado pela marcação de administrador gravada nos metadados do usuário, que somente o servidor pode alterar.

- Estabelecimento: realiza o login informando e-mail e senha. O Supabase Auth valida as credenciais, guarda a senha de forma segura em sua própria tabela de usuários e disponibiliza o token de acesso. O backend localiza o estabelecimento pelo campo user_id da tabela establishments. O CPF ou CNPJ é informado apenas no cadastro e não faz parte do login.

- Operador de bar: realiza o login no app nativo informando o código de acesso individual. Como o operador não possui conta no Supabase Auth, o próprio backend valida o código na tabela operators, verifica se o campo is_active é verdadeiro e se o evento correspondente ainda não terminou, e emite um token próprio, assinado pelo backend, que expira no fim do evento. A cada requisição, o backend confere novamente se o operador continua ativo, de modo que a desativação feita pelo estabelecimento tem efeito imediato. O app do operador se comunica exclusivamente com o backend.

- Cliente: não cria conta nem faz login. No primeiro acesso, o aplicativo cria uma sessão anônima no Supabase Auth de forma invisível, sem nenhuma tela ou dado pessoal. Os pedidos ficam vinculados a essa sessão, o que permite exibir os avisos de pedidos em aberto e atualizar o comprovante em tempo real apenas para o celular que fez o pedido. A criação dessas sessões é protegida por um CAPTCHA invisível, e uma rotina diária exclui as sessões sem pedidos em aberto e sem atividade há mais de 24 horas, desvinculando-as dos pedidos antes da exclusão.

Todas as operações que gravam dados, como criar eventos, cardápios e pedidos, iniciar pagamentos, ler QR Codes e registrar estornos, passam pelo backend, que concentra as regras de negócio e acessa o banco por meio do Prisma. Os aplicativos acessam o Supabase diretamente em apenas dois casos: a autenticação pelo Supabase Auth e o recebimento de atualizações em tempo real pelo Supabase Realtime. As políticas de segurança por linha (RLS) ficam ativas em todas as tabelas e liberam somente a leitura necessária para o tempo real: o estabelecimento lê os pedidos dos próprios eventos e cada sessão anônima lê apenas os próprios pedidos.

## 7.4 Fluxo de Pagamento

O processamento de pagamentos é realizado pelo Pagar.me, gateway escolhido por sua robustez, suporte nativo ao Pix e pela funcionalidade de split automático, que distribui os valores entre os recebedores sem necessidade de nenhuma intervenção manual. Como a confirmação do Pix é assíncrona, o pedido é criado antes do pagamento e só libera o QR Code de retirada depois que o backend recebe a confirmação oficial do Pagar.me. O fluxo completo ocorre da seguinte forma:

- O consumidor confirma o carrinho e o backend cria o pedido com o status awaiting_payment, calculando o valor total a partir dos preços do cardápio, e não de valores enviados pelo aplicativo.

- O consumidor escolhe a forma de pagamento e o backend cria uma tentativa de pagamento na tabela payments, acionando a API do Pagar.me com as regras de split. No cartão, os dados são tokenizados diretamente pelo Pagar.me no aplicativo, e o número do cartão nunca passa pelo backend nem é armazenado.

- O Pagar.me informa o resultado ao backend por webhook. Cada aviso recebido é registrado na tabela payment_webhooks; se o mesmo aviso chegar mais de uma vez, ele é identificado e não é processado novamente, o que torna a operação idempotente.

- Com o pagamento aprovado, o pedido passa para paid e o QR Code de retirada é exibido na tela do consumidor. A confirmação nunca é aceita com base em informação enviada pelo aplicativo.

- Se uma tentativa falhar, como um cartão recusado, apenas a tentativa fica com o status failed. O pedido continua aguardando pagamento e o consumidor pode tentar novamente, com outro cartão ou com Pix. Se nenhuma tentativa for aprovada em 15 minutos, o pedido passa para expired e nenhum valor é cobrado.

- No split, a plataforma recebe 5% do valor total do pedido. O estabelecimento recebe os 95% restantes, descontada a taxa do Pagar.me, que varia conforme o meio de pagamento e o contrato. Eventuais chargebacks também ficam a cargo do estabelecimento.

- Quando um item acaba, o operador registra um estorno parcial ou total. O backend solicita o estorno ao Pagar.me, que devolve o valor pelo mesmo meio de pagamento, e o split é desfeito na proporção do valor estornado. Conforme a documentação do Pagar.me, a taxa do Pix e as taxas fixas do cartão não são devolvidas no estorno, e esse custo fica com o estabelecimento.

| **Status do pedido** | **Significado**                                               |
|----------------------|---------------------------------------------------------------|
| awaiting_payment     | Pedido criado, aguardando a confirmação do pagamento          |
| paid                 | Pagamento confirmado; QR Code de retirada válido              |
| in_service           | QR Code lido; pedido reservado para o operador que o escaneou |
| collected            | Entrega confirmada; QR Code invalidado e pedido encerrado     |
| refunded             | Pedido estornado integralmente                                |
| expired              | Nenhum pagamento aprovado dentro do prazo; nada foi cobrado   |

As transições possíveis são: awaiting_payment para paid ou expired; paid para in_service; in_service para collected, refunded ou de volta para paid, quando o atendimento é cancelado. Estornos parciais não alteram o status: o valor devolvido é registrado no campo refunded_amount e na tabela refunds, e o pedido segue para collected com os itens restantes.

## 7.5 Rotas da API

A API do backend segue o padrão REST, com todas as rotas sob o prefixo /api/v1, dados em formato JSON e validação dos dados de entrada com Zod. As rotas protegidas recebem o token no cabeçalho Authorization. A coluna Acesso indica quem pode chamar cada rota: Público (sem autenticação), Consumidor (sessão anônima), Operador (token emitido pelo backend), Estabelecimento (Supabase Auth, somente para recursos próprios), Admin (Supabase Auth com marcação de administrador) e Pagar.me (assinatura do webhook verificada).

### 7.5.1 Rotas públicas e do consumidor

| **Método** | **Rota**                            | **Acesso** | **Descrição**                                                  |
|------------|-------------------------------------|------------|----------------------------------------------------------------|
| GET        | /public/establishments/:slug/events | Público    | Eventos publicados em andamento e que começam em breve         |
| GET        | /public/events/:eventId/menu        | Público    | Cardápio do evento, com disponibilidade dos itens              |
| POST       | /orders                             | Consumidor | Cria o pedido com status awaiting_payment                      |
| POST       | /orders/:orderId/payments           | Consumidor | Inicia uma tentativa de pagamento via Pix ou cartão tokenizado |
| GET        | /orders/open                        | Consumidor | Pedidos em aberto da sessão, usados nos avisos do cardápio     |
| GET        | /orders/:orderId                    | Consumidor | Comprovante do pedido com status atual                         |

### 7.5.2 Rotas do operador

| **Método** | **Rota**                          | **Acesso** | **Descrição**                                            |
|------------|-----------------------------------|------------|----------------------------------------------------------|
| POST       | /operator/login                   | Público    | Valida o código de acesso e emite o token do operador    |
| GET        | /operator/me                      | Operador   | Dados do operador e atendimento em aberto, para retomada |
| POST       | /operator/orders/scan             | Operador   | Lê o QR Code e reserva o pedido (paid para in_service)   |
| POST       | /operator/orders/:orderId/confirm | Operador   | Confirma a entrega (in_service para collected)           |
| POST       | /operator/orders/:orderId/release | Operador   | Cancela o atendimento (in_service para paid)             |
| POST       | /operator/orders/:orderId/refunds | Operador   | Estorna itens esgotados ou o pedido inteiro              |
| GET        | /operator/orders/delivered        | Operador   | Pedidos entregues pelo operador no evento                |

### 7.5.3 Rotas do estabelecimento

| **Método** | **Rota**                                    | **Acesso**      | **Descrição**                                               |
|------------|---------------------------------------------|-----------------|-------------------------------------------------------------|
| POST       | /establishments                             | Estabelecimento | Completa o cadastro: nome, slug, CPF/CNPJ e dados bancários |
| GET        | /establishments/me                          | Estabelecimento | Dados do estabelecimento e status de aprovação              |
| PATCH      | /establishments/me                          | Estabelecimento | Atualiza dados cadastrais; slug só antes da aprovação       |
| GET        | /establishments/me/access                   | Estabelecimento | Link e QR Code fixo; disponível após a aprovação            |
| GET        | /menus                                      | Estabelecimento | Lista os cardápios reutilizáveis                            |
| POST       | /menus                                      | Estabelecimento | Cria um cardápio                                            |
| PATCH      | /menus/:menuId                              | Estabelecimento | Renomeia um cardápio                                        |
| DELETE     | /menus/:menuId                              | Estabelecimento | Exclui um cardápio sem afetar eventos já criados            |
| POST       | /menus/:menuId/items                        | Estabelecimento | Adiciona item ao cardápio                                   |
| PATCH      | /menus/:menuId/items/:itemId                | Estabelecimento | Altera item do cardápio                                     |
| DELETE     | /menus/:menuId/items/:itemId                | Estabelecimento | Remove item do cardápio                                     |
| GET        | /events                                     | Estabelecimento | Lista os eventos com status calculado                       |
| POST       | /events                                     | Estabelecimento | Cria evento e copia os itens do cardápio escolhido          |
| PATCH      | /events/:eventId                            | Estabelecimento | Altera nome e horários do evento                            |
| POST       | /events/:eventId/publish                    | Estabelecimento | Publica o evento; exige cadastro aprovado                   |
| POST       | /events/:eventId/menu-items                 | Estabelecimento | Adiciona item apenas a este evento                          |
| PATCH      | /events/:eventId/menu-items/:itemId         | Estabelecimento | Altera item do evento, inclusive disponível ou esgotado     |
| DELETE     | /events/:eventId/menu-items/:itemId         | Estabelecimento | Remove item do evento                                       |
| GET        | /events/:eventId/operators                  | Estabelecimento | Lista os operadores do evento                               |
| POST       | /events/:eventId/operators                  | Estabelecimento | Cadastra operador e retorna o código uma única vez          |
| PATCH      | /events/:eventId/operators/:operatorId      | Estabelecimento | Altera balcão ou ativa e desativa o acesso                  |
| POST       | /events/:eventId/operators/:operatorId/code | Estabelecimento | Gera novo código e invalida o anterior                      |
| GET        | /events/:eventId/orders                     | Estabelecimento | Pedidos do evento; atualizações chegam pelo Realtime        |
| GET        | /events/:eventId/dashboard                  | Estabelecimento | Faturamento, pedidos, ticket médio e itens mais vendidos    |
| GET        | /events/:eventId/refunds                    | Estabelecimento | Estornos do evento, para auditoria                          |

### 7.5.4 Rotas do administrador

| **Método** | **Rota**                          | **Acesso** | **Descrição**                                                                                |
|------------|-----------------------------------|------------|----------------------------------------------------------------------------------------------|
| GET        | /admin/overview                   | Admin      | Resumo do dia: estabelecimentos ativos, eventos ao vivo, pedidos e receita                   |
| GET        | /admin/billing?month=AAAA-MM      | Admin      | Faturamento do mês: vendas, transacionado, estornos e receita, por dia e por estabelecimento |
| GET        | /admin/orders?code=:orderCode     | Admin      | Busca pedidos pelo código em todos os eventos, com pagamentos, estornos e histórico          |
| GET        | /admin/establishments             | Admin      | Lista cadastros, com filtro por status de aprovação                                          |
| POST       | /admin/establishments/:id/approve | Admin      | Aprova o cadastro e libera a publicação de eventos                                           |
| POST       | /admin/establishments/:id/reject  | Admin      | Rejeita o cadastro                                                                           |
| PATCH      | /admin/establishments/:id/slug    | Admin      | Altera o slug em caso excepcional                                                            |

### 7.5.5 Integrações e sistema

| **Método** | **Rota**          | **Acesso** | **Descrição**                                             |
|------------|-------------------|------------|-----------------------------------------------------------|
| POST       | /webhooks/pagarme | Pagar.me   | Recebe avisos de pagamento e estorno de forma idempotente |
| GET        | /health           | Público    | Verificação de funcionamento do backend                   |

# 8 DICIONÁRIO DE DADOS

O dicionário de dados descreve a estrutura completa do banco de dados relacional da plataforma, especificando cada tabela, seus campos, tipos de dados, obrigatoriedade e descrição funcional. Toda a nomenclatura segue o padrão universal snake_case em inglês, adotado como padrão profissional em sistemas de banco de dados. Chaves primárias são sempre nomeadas id, chaves estrangeiras seguem o padrão {tabela_singular}\_id, campos booleanos utilizam o prefixo is\_ e campos de data e hora utilizam o sufixo \_at e o tipo TIMESTAMPTZ, que registra o fuso horário. Todas as tabelas possuem os campos created_at e updated_at, exceto payment_webhooks, que é um registro imutável.

O banco de dados é composto por doze tabelas. Um estabelecimento possui cardápios reutilizáveis e múltiplos eventos; cada evento possui uma cópia própria dos itens do cardápio, seus operadores e seus pedidos; cada pedido é composto por um ou mais itens e pode ter várias tentativas de pagamento e estornos. As tabelas de usuários ficam no Supabase Auth (auth.users) e não são gerenciadas pelo Prisma: a ligação com elas é criada por uma migration SQL própria. As políticas de segurança por linha (RLS) ficam ativas em todas as tabelas.

## 8.1 Tabela: establishments

Armazena os dados cadastrais de cada estabelecimento registrado na plataforma. É a entidade raiz do sistema, à qual todos os cardápios, eventos e operações estão vinculados. O e-mail e a senha de acesso ficam no Supabase Auth, e não nesta tabela.

| **Campo**            | **Tipo**    | **Obrigatório** | **Descrição**                                                                                                                                                     |
|----------------------|-------------|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id                   | UUID        | Sim             | Identificador único do estabelecimento, gerado automaticamente pelo banco de dados                                                                                |
| user_id              | UUID        | Sim             | Referência ao usuário do Supabase Auth dono do estabelecimento. Chave estrangeira para auth.users. Deve ser único                                                 |
| name                 | TEXT        | Sim             | Nome do estabelecimento, conforme registrado para exibição na plataforma                                                                                          |
| slug                 | TEXT        | Sim             | Identificador público usado no link e no QR Code fixo, com letras minúsculas, números e hífen. Único; não pode ser alterado pelo estabelecimento após a aprovação |
| document_type        | TEXT        | Sim             | Tipo do documento informado no cadastro: cpf ou cnpj                                                                                                              |
| document             | TEXT        | Sim             | CPF ou CNPJ, somente números, com dígitos verificadores validados no cadastro. Deve ser único no sistema                                                          |
| approval_status      | TEXT        | Sim             | Situação do cadastro: pending (aguardando), approved (aprovado) ou rejected (rejeitado)                                                                           |
| approved_at          | TIMESTAMPTZ | Não             | Data e hora da aprovação pelo administrador                                                                                                                       |
| pagarme_recipient_id | TEXT        | Não             | Identificador do recebedor no Pagar.me, usado no split. Os dados bancários ficam apenas no Pagar.me                                                               |
| created_at           | TIMESTAMPTZ | Sim             | Data e hora de cadastro do estabelecimento na plataforma                                                                                                          |
| updated_at           | TIMESTAMPTZ | Sim             | Data e hora da última alteração do registro                                                                                                                       |

## 8.2 Tabela: menus

Armazena os cardápios reutilizáveis de cada estabelecimento. Um cardápio funciona como modelo: ao criar um evento, seus itens são copiados para o evento, de modo que alterar ou excluir o cardápio não afeta eventos já criados.

| **Campo**        | **Tipo**    | **Obrigatório** | **Descrição**                                                                                  |
|------------------|-------------|-----------------|------------------------------------------------------------------------------------------------|
| id               | UUID        | Sim             | Identificador único do cardápio                                                                |
| establishment_id | UUID        | Sim             | Referência ao estabelecimento dono do cardápio. Chave estrangeira para a tabela establishments |
| name             | TEXT        | Sim             | Nome do cardápio, por exemplo "Cardápio Forró"                                                 |
| created_at       | TIMESTAMPTZ | Sim             | Data e hora de criação do cardápio                                                             |
| updated_at       | TIMESTAMPTZ | Sim             | Data e hora da última alteração do registro                                                    |

## 8.3 Tabela: menu_template_items

Armazena os itens de cada cardápio reutilizável. Não possui controle de disponibilidade, que existe apenas nos itens do evento.

| **Campo**  | **Tipo**    | **Obrigatório** | **Descrição**                                                                         |
|------------|-------------|-----------------|---------------------------------------------------------------------------------------|
| id         | UUID        | Sim             | Identificador único do item do cardápio modelo                                        |
| menu_id    | UUID        | Sim             | Referência ao cardápio ao qual o item pertence. Chave estrangeira para a tabela menus |
| name       | TEXT        | Sim             | Nome do item                                                                          |
| price      | DECIMAL     | Sim             | Preço unitário sugerido do item                                                       |
| category   | TEXT        | Sim             | Categoria fixa do item: beverage (Bebidas), snack (Petiscos) ou food (Comidas)        |
| emoji      | TEXT        | Sim             | Emoji utilizado como ícone visual do item                                             |
| created_at | TIMESTAMPTZ | Sim             | Data e hora de criação do item                                                        |
| updated_at | TIMESTAMPTZ | Sim             | Data e hora da última alteração do registro                                           |

## 8.4 Tabela: events

Armazena os dados de cada evento criado por um estabelecimento. O status do evento não é armazenado: ele é calculado a partir de published_at, starts_at, ends_at e do horário atual. Sem published_at, o evento é um rascunho e não aparece para o consumidor; publicado e antes de starts_at, está agendado e aparece como "Começa às 21h"; entre starts_at e ends_at, está ativo, com pedidos e operadores liberados; após ends_at, está encerrado, e os QR Codes e códigos de operador expiram.

| **Campo**        | **Tipo**    | **Obrigatório** | **Descrição**                                                                                                                                                                          |
|------------------|-------------|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id               | UUID        | Sim             | Identificador único do evento, gerado automaticamente pelo banco de dados                                                                                                              |
| establishment_id | UUID        | Sim             | Referência ao estabelecimento dono do evento. Chave estrangeira para a tabela establishments                                                                                           |
| name             | TEXT        | Sim             | Nome do evento, exibido na tela de seleção e no painel do estabelecimento                                                                                                              |
| starts_at        | TIMESTAMPTZ | Sim             | Data e hora de início do evento. Define quando o cardápio é liberado para pedidos                                                                                                      |
| ends_at          | TIMESTAMPTZ | Sim             | Data e hora de término do evento, que pode ser no dia seguinte ao início. Define quando os QR Codes e os códigos de operador expiram. Deve ser posterior a starts_at (restrição CHECK) |
| published_at     | TIMESTAMPTZ | Não             | Data e hora em que o estabelecimento publicou o evento. Vazio enquanto o evento é um rascunho                                                                                          |
| created_at       | TIMESTAMPTZ | Sim             | Data e hora de criação do evento no sistema                                                                                                                                            |
| updated_at       | TIMESTAMPTZ | Sim             | Data e hora da última alteração do registro                                                                                                                                            |

## 8.5 Tabela: menu_items

Armazena os itens do cardápio de cada evento. Os itens são copiados do cardápio escolhido na criação do evento e podem ser ajustados apenas para aquela ocasião.

| **Campo**    | **Tipo**    | **Obrigatório** | **Descrição**                                                                                                                         |
|--------------|-------------|-----------------|---------------------------------------------------------------------------------------------------------------------------------------|
| id           | UUID        | Sim             | Identificador único do item do cardápio do evento                                                                                     |
| event_id     | UUID        | Sim             | Referência ao evento ao qual o item pertence. Chave estrangeira para a tabela events                                                  |
| name         | TEXT        | Sim             | Nome do item, exibido no cardápio para o consumidor                                                                                   |
| price        | DECIMAL     | Sim             | Preço unitário do item no evento                                                                                                      |
| category     | TEXT        | Sim             | Categoria fixa do item: beverage (Bebidas), snack (Petiscos) ou food (Comidas). Define o agrupamento e a ordem no cardápio            |
| emoji        | TEXT        | Sim             | Emoji utilizado como ícone visual do item no cardápio                                                                                 |
| is_available | BOOLEAN     | Sim             | Indica se o item está disponível. Alterado para false quando o operador marca o item como esgotado; o estabelecimento pode reativá-lo |
| created_at   | TIMESTAMPTZ | Sim             | Data e hora de criação do item                                                                                                        |
| updated_at   | TIMESTAMPTZ | Sim             | Data e hora da última alteração do registro                                                                                           |

## 8.6 Tabela: operators

Armazena os operadores de bar cadastrados para cada evento. Cada operador possui um código de acesso individual que lhe permite autenticar-se no aplicativo nativo durante o evento.

| **Campo**        | **Tipo**    | **Obrigatório** | **Descrição**                                                                                                                                                                                  |
|------------------|-------------|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id               | UUID        | Sim             | Identificador único do operador                                                                                                                                                                |
| event_id         | UUID        | Sim             | Referência ao evento para o qual o operador foi cadastrado. Chave estrangeira para a tabela events                                                                                             |
| name             | TEXT        | Sim             | Nome do operador, utilizado para identificação no painel do estabelecimento                                                                                                                    |
| counter_name     | TEXT        | Não             | Nome do balcão em que o operador atua, por exemplo "Bar Principal". Apenas informativo, sem restringir quais pedidos o operador pode atender                                                   |
| access_code_hash | TEXT        | Sim             | Hash do código de acesso de seis caracteres, calculado com HMAC-SHA-256 e uma chave secreta do servidor. O código em texto puro é exibido uma única vez e nunca é armazenado. Único no sistema |
| is_active        | BOOLEAN     | Sim             | Indica se o acesso do operador está ativo. O estabelecimento pode revogar o acesso alterando este campo para false                                                                             |
| created_at       | TIMESTAMPTZ | Sim             | Data e hora de cadastro do operador                                                                                                                                                            |
| updated_at       | TIMESTAMPTZ | Sim             | Data e hora da última alteração do registro, incluindo a geração de um novo código                                                                                                             |

## 8.7 Tabela: orders

Armazena os pedidos realizados pelos consumidores durante um evento. É a tabela central do sistema, conectando o evento, a sessão do consumidor, os pagamentos e o operador que realizou a entrega.

| **Campo**       | **Tipo**    | **Obrigatório** | **Descrição**                                                                                                           |
|-----------------|-------------|-----------------|-------------------------------------------------------------------------------------------------------------------------|
| id              | UUID        | Sim             | Identificador único do pedido, utilizado também como conteúdo do QR Code do comprovante                                 |
| event_id        | UUID        | Sim             | Referência ao evento em que o pedido foi realizado. Chave estrangeira para a tabela events                              |
| client_id       | UUID        | Não             | Referência à sessão anônima do consumidor no Supabase Auth. Esvaziado quando a sessão é excluída pela rotina de limpeza |
| operator_id     | UUID        | Não             | Referência ao operador que leu o QR Code e realizou o atendimento. Chave estrangeira para a tabela operators            |
| order_code      | TEXT        | Sim             | Código alfanumérico exibido no comprovante do consumidor para identificação visual do pedido                            |
| total_amount    | DECIMAL     | Sim             | Valor total do pedido, calculado pelo backend com base nos itens e quantidades no momento da compra                     |
| refunded_amount | DECIMAL     | Sim             | Soma dos valores estornados ao consumidor. Inicia em zero                                                               |
| status          | TEXT        | Sim             | Estado atual do pedido: awaiting_payment, paid, in_service, collected, refunded ou expired                              |
| paid_at         | TIMESTAMPTZ | Não             | Data e hora em que o pagamento foi confirmado pelo Pagar.me                                                             |
| in_service_at   | TIMESTAMPTZ | Não             | Data e hora em que o operador leu o QR Code                                                                             |
| collected_at    | TIMESTAMPTZ | Não             | Data e hora em que a entrega foi confirmada                                                                             |
| created_at      | TIMESTAMPTZ | Sim             | Data e hora em que o pedido foi criado. Exibido no comprovante do consumidor                                            |
| updated_at      | TIMESTAMPTZ | Sim             | Data e hora da última alteração do registro                                                                             |

## 8.8 Tabela: order_items

Armazena os itens individuais de cada pedido. Como um pedido pode conter múltiplos itens, esta tabela faz a ligação entre um pedido e os itens do cardápio que o compõem, registrando também o preço unitário no momento da compra para garantir que variações futuras no cardápio não alterem o histórico de pedidos.

| **Campo**         | **Tipo**    | **Obrigatório** | **Descrição**                                                                                                   |
|-------------------|-------------|-----------------|-----------------------------------------------------------------------------------------------------------------|
| id                | UUID        | Sim             | Identificador único do item do pedido                                                                           |
| order_id          | UUID        | Sim             | Referência ao pedido ao qual o item pertence. Chave estrangeira para a tabela orders                            |
| menu_item_id      | UUID        | Sim             | Referência ao item do cardápio solicitado. Chave estrangeira para a tabela menu_items                           |
| quantity          | INTEGER     | Sim             | Quantidade do item solicitada pelo consumidor neste pedido                                                      |
| unit_price        | DECIMAL     | Sim             | Preço unitário do item no momento em que o pedido foi realizado, independente de alterações futuras no cardápio |
| refunded_quantity | INTEGER     | Sim             | Quantidade deste item que foi estornada. Inicia em zero                                                         |
| created_at        | TIMESTAMPTZ | Sim             | Data e hora de criação do registro                                                                              |
| updated_at        | TIMESTAMPTZ | Sim             | Data e hora da última alteração do registro                                                                     |

## 8.9 Tabela: payments

Armazena cada tentativa de pagamento de um pedido. Um pedido pode ter várias tentativas, por exemplo um cartão recusado seguido de um Pix aprovado, e todas ficam registradas.

| **Campo**         | **Tipo**    | **Obrigatório** | **Descrição**                                                                   |
|-------------------|-------------|-----------------|---------------------------------------------------------------------------------|
| id                | UUID        | Sim             | Identificador único da tentativa de pagamento                                   |
| order_id          | UUID        | Sim             | Referência ao pedido. Chave estrangeira para a tabela orders                    |
| method            | TEXT        | Sim             | Forma de pagamento da tentativa: pix ou credit_card                             |
| status            | TEXT        | Sim             | Estado da tentativa: pending, paid, failed ou expired                           |
| amount            | DECIMAL     | Sim             | Valor cobrado na tentativa                                                      |
| pagarme_order_id  | TEXT        | Sim             | Identificador do pedido no Pagar.me                                             |
| pagarme_charge_id | TEXT        | Não             | Identificador da cobrança no Pagar.me, usado nos webhooks e nos estornos. Único |
| expires_at        | TIMESTAMPTZ | Não             | Prazo de pagamento do Pix (15 minutos). Não se aplica ao cartão                 |
| paid_at           | TIMESTAMPTZ | Não             | Data e hora da aprovação da tentativa                                           |
| created_at        | TIMESTAMPTZ | Sim             | Data e hora de criação da tentativa                                             |
| updated_at        | TIMESTAMPTZ | Sim             | Data e hora da última alteração do registro                                     |

## 8.10 Tabela: payment_webhooks

Registra cada aviso recebido do Pagar.me. Garante a idempotência: um aviso repetido é reconhecido pelo identificador do evento e não é processado novamente.

| **Campo**        | **Tipo**    | **Obrigatório** | **Descrição**                                             |
|------------------|-------------|-----------------|-----------------------------------------------------------|
| id               | UUID        | Sim             | Identificador único do registro                           |
| pagarme_event_id | TEXT        | Sim             | Identificador do aviso enviado pelo Pagar.me. Único       |
| event_type       | TEXT        | Sim             | Tipo do aviso, por exemplo charge.paid ou charge.refunded |
| payload          | JSONB       | Sim             | Conteúdo completo do aviso, para auditoria                |
| processed_at     | TIMESTAMPTZ | Não             | Data e hora em que o aviso foi processado com sucesso     |
| created_at       | TIMESTAMPTZ | Sim             | Data e hora de recebimento do aviso                       |

## 8.11 Tabela: refunds

Registra cada estorno realizado, parcial ou total, com o operador responsável, permitindo a auditoria pelo estabelecimento.

| **Campo**   | **Tipo**    | **Obrigatório** | **Descrição**                                                                                             |
|-------------|-------------|-----------------|-----------------------------------------------------------------------------------------------------------|
| id          | UUID        | Sim             | Identificador único do estorno                                                                            |
| order_id    | UUID        | Sim             | Referência ao pedido estornado. Chave estrangeira para a tabela orders                                    |
| payment_id  | UUID        | Sim             | Referência à tentativa de pagamento aprovada que será estornada. Chave estrangeira para a tabela payments |
| operator_id | UUID        | Sim             | Referência ao operador que registrou o estorno. Chave estrangeira para a tabela operators                 |
| amount      | DECIMAL     | Sim             | Valor devolvido ao consumidor                                                                             |
| reason      | TEXT        | Sim             | Motivo do estorno: item_sold_out (item esgotado) ou order_cancelled (pedido inteiro estornado)            |
| status      | TEXT        | Sim             | Situação do estorno no Pagar.me: pending, completed ou failed                                             |
| created_at  | TIMESTAMPTZ | Sim             | Data e hora em que o estorno foi registrado                                                               |
| updated_at  | TIMESTAMPTZ | Sim             | Data e hora da última alteração do registro                                                               |

## 8.12 Tabela: refund_items

Registra quais itens e quantidades fizeram parte de cada estorno.

| **Campo**     | **Tipo**    | **Obrigatório** | **Descrição**                                                                       |
|---------------|-------------|-----------------|-------------------------------------------------------------------------------------|
| id            | UUID        | Sim             | Identificador único do registro                                                     |
| refund_id     | UUID        | Sim             | Referência ao estorno. Chave estrangeira para a tabela refunds                      |
| order_item_id | UUID        | Sim             | Referência ao item do pedido estornado. Chave estrangeira para a tabela order_items |
| quantity      | INTEGER     | Sim             | Quantidade estornada do item                                                        |
| amount        | DECIMAL     | Sim             | Valor estornado referente ao item                                                   |
| created_at    | TIMESTAMPTZ | Sim             | Data e hora de criação do registro                                                  |
| updated_at    | TIMESTAMPTZ | Sim             | Data e hora da última alteração do registro                                         |

# 9 MODELO DE NEGÓCIO

## 9.1 Modelo de Receita

A plataforma adota o modelo de taxa por transação, cobrando 5% sobre o valor total de cada pedido realizado. Esse percentual é descontado automaticamente a cada transação via split do Pagar.me, sem necessidade de nenhuma ação manual de cobrança. A taxa do meio de pagamento cobrada pelo Pagar.me e eventuais chargebacks ficam a cargo do estabelecimento, que recebe os 95% restantes, da mesma forma que já ocorre com as máquinas de cartão. O modelo foi escolhido por três razões principais: elimina completamente a barreira de entrada para o estabelecimento, que não paga nada adiantado para começar a usar a plataforma; alinha diretamente os interesses da plataforma com os do estabelecimento, pois ambos só ganham quando o evento acontece e gera pedidos; e é praticamente invisível para o consumidor final, representando menos de R\$ 1,00 a cada R\$ 20,00 em compras.

## 9.2 Projeção de Receita

A tabela a seguir apresenta projeções de receita para diferentes cenários de operação, considerando um único estabelecimento com frequência de eventos semanal:

| **Cenário** | **Volume por evento** | **Eventos por semana** | **Faturamento mensal do estabelecimento** | **Receita mensal da plataforma** |
|-------------|-----------------------|------------------------|-------------------------------------------|----------------------------------|
| Conservador | R\$ 3.000             | 2                      | R\$ 24.000                                | R\$ 1.200                        |
| Realista    | R\$ 5.000             | 2                      | R\$ 40.000                                | R\$ 2.000                        |
| Cheio       | R\$ 8.000             | 3                      | R\$ 96.000                                | R\$ 4.800                        |

A escalabilidade do modelo se evidencia na expansão para múltiplos estabelecimentos. Com 10 clientes ativos com perfil similar ao cenário realista, a receita mensal da plataforma pode chegar a R\$ 20.000, de forma totalmente automatizada e sem custos operacionais adicionais significativos. Os valores apresentados são brutos, isto é, anteriores à dedução de impostos e dos custos de infraestrutura, que dependem do regime tributário adotado e do volume de uso.

## 9.3 Estratégia de Crescimento

A estratégia de crescimento foi estruturada em quatro fases progressivas, cada uma construindo sobre os resultados da anterior:

| **Fase**                   | **Estratégia**                                                                                     | **Objetivo**                                                                   |
|----------------------------|----------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------|
| Fase 1 — Validação         | Lançamento com primeiros estabelecimentos parceiros em ambiente real de evento                     | Validar o produto, identificar ajustes e construir o primeiro caso de sucesso  |
| Fase 2 — Expansão regional | Crescimento orgânico por indicação entre estabelecimentos do mesmo segmento e região               | Construir base de clientes sem custo de aquisição                              |
| Fase 3 — Novos segmentos   | Expansão para festas universitárias, feiras, eventos corporativos e culturais de diferentes portes | Diversificar a base de clientes e reduzir dependência de um único segmento     |
| Fase 4 — Integração        | Desenvolvimento de integrações com plataformas de venda de ingressos como Sympla e Ingresso.com    | Tornar a plataforma parte do ecossistema de eventos desde a compra do ingresso |

# 10 PROTÓTIPOS

Foram desenvolvidos três protótipos interativos navegáveis em HTML, representando as principais interfaces da plataforma. Os protótipos simulam o fluxo real de uso de cada perfil, permitindo que qualquer pessoa possa visualizar e interagir com a solução antes do início do desenvolvimento.

- Protótipo do consumidor e operador: simula o fluxo completo do consumidor, desde a tela de seleção de evento, passando pelo cardápio com itens esgotados, pelo pagamento via Pix com QR Code, código copia e cola e contagem regressiva, e pelo formulário de cartão com tratamento de recusa, até o comprovante com QR Code e status em tempo real. Inclui os avisos de pedido em aberto no topo do cardápio. Do lado do operador, simula o login com código, o scanner com câmera simulada, o pedido reservado com as ações de confirmar entrega, cancelar atendimento e marcar item esgotado com estorno, e as mensagens de erro de leitura.

- Protótipo do estabelecimento: simula o login e o cadastro com CPF ou CNPJ, o aviso de cadastro aguardando aprovação, o dashboard de faturamento em tempo real, a listagem de eventos com status calculado, a criação de eventos a partir de cardápios reutilizáveis e a publicação, o gerenciamento de cardápios e dos itens do evento com controle de disponibilidade, o cadastro de operadores com código exibido uma única vez e envio pelo WhatsApp, a tela de link e QR Code fixo, o acompanhamento de pedidos ao vivo e a auditoria de estornos.

- Protótipo do administrador: simula o login, a visão geral do dia, o faturamento mensal com gráfico de vendas por dia e tabela por estabelecimento, a busca de pedido pelo código com o histórico completo e a análise de cadastros, com as listas de cadastros pendentes, aprovados e rejeitados, os dados e as verificações de cada estabelecimento, a aprovação e a rejeição com confirmação e a alteração excepcional do endereço do link, com o aviso de que os QR Codes já impressos deixam de funcionar.

Os protótipos foram desenvolvidos com identidade visual própria, utilizando a fonte Space Grotesk para elementos de destaque, como nome do evento e códigos de pedido, e Inter para textos de interface. O tema claro com azul principal (#2B6CB0), fundo das telas em cinza muito claro (#F7FAFC) e cards brancos foi escolhido para garantir legibilidade em ambientes com baixa iluminação, característicos de eventos noturnos como shows e festas.

# 11 ROADMAP DE DESENVOLVIMENTO

O roadmap de desenvolvimento define as entregas planejadas em cada fase do produto, organizadas de forma a priorizar o que é essencial para validação no mundo real antes de evoluir para funcionalidades mais complexas:

| **Fase** | **Entregas previstas**                                                                                                                                                                                                                                                                                                         | **Objetivo da fase**                                                    |
|----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| MVP      | QR Code fixo do estabelecimento, tela de seleção de evento, cardápios reutilizáveis, pagamento via Pix e cartão, QR Code do comprovante, app do operador com scanner e estorno de itens esgotados, painel do estabelecimento básico, painel do administrador com aprovação de cadastros, faturamento mensal e busca de pedidos | Validar a solução em um ambiente real de evento                         |
| Fase 2   | Dashboard completo com gráficos, relatórios por período, histórico de eventos, busca e filtro de operadores, métricas individuais por operador, status online e offline dos operadores, categorias de cardápio personalizáveis                                                                                                 | Aprimorar a experiência de gestão do estabelecimento                    |
| Fase 3   | Suporte a múltiplos balcões com cardápios distintos, notificações push para operadores, envio automático do código de acesso por SMS ou WhatsApp, migração para infraestrutura escalável conforme demanda                                                                                                                      | Preparar a plataforma para operações de maior porte                     |
| Fase 4   | Integração com plataformas de venda de ingressos, desenvolvimento de app nativo para o consumidor                                                                                                                                                                                                                              | Expandir o alcance e a presença da plataforma no ecossistema de eventos |

# 12 CONSIDERAÇÕES FINAIS

A plataforma digital de pedidos para eventos nasce da identificação de uma dor real, recorrente e amplamente reconhecida no mercado de entretenimento brasileiro — um dos mais dinâmicos e em acelerada expansão do mundo. A solução proposta é tecnicamente viável, construída sobre uma stack moderna e amplamente utilizada no mercado profissional, com custo zero na fase de desenvolvimento e um modelo de receita escalável que só exige investimento quando já está gerando retorno.

A decisão de utilizar um QR Code fixo por estabelecimento, em vez de gerar um novo a cada evento, representa um diferencial operacional e econômico significativo para os clientes da plataforma. Essa escolha elimina um custo recorrente real, reduz a fricção de adoção e demonstra que a solução foi pensada com profundo entendimento das necessidades de quem organiza eventos no dia a dia.

A estruturação cuidadosa do dicionário de dados, com nomenclatura padronizada em inglês e snake_case, e a definição de uma arquitetura técnica que evita retrabalho futuro, refletem o compromisso com a construção de uma plataforma limpa, profissional e preparada para crescer. O banco de dados relacional no Supabase, combinado com o backend em Node.js e TypeScript, garante que a plataforma possa evoluir de forma sustentável sem necessidade de reestruturações fundamentais.

O próximo passo é o desenvolvimento do MVP e sua aplicação em um evento real, permitindo coleta de feedback direto dos usuários — estabelecimento, operadores e consumidores — e a realização dos ajustes necessários com base em dados reais de uso.

# REFERÊNCIAS

ABRAPE. ABRAPE prevê R\$ 141,1 bilhões em consumo e forte expansão de empregos no setor de eventos em 2025. 21 jan. 2025. Disponível em: https://www.abrape.com.br/abrape-preve-r-141-bilhoes-em-consumo-e-forte-expansao-de-empregos-no-setor-de-eventos-em-2025/. Acesso em: 12 ago. 2026.

ASSOCIAÇÃO BRASILEIRA DE NORMAS TÉCNICAS. ABNT NBR 14724: informação e documentação — trabalhos acadêmicos — apresentação. 4. ed. Rio de Janeiro: ABNT, 2024.

ASSOCIAÇÃO BRASILEIRA DE NORMAS TÉCNICAS. ABNT NBR 6023: informação e documentação — referências — elaboração. 3. ed. Rio de Janeiro: ABNT, 2025.

CENTRO PAULA SOUZA. Gestão de Eventos. Disponível em: https://www.cps.sp.gov.br/cursos-fatec/eventos/. Acesso em: 12 ago. 2026.

FLUTTER. Flutter documentation. Disponível em: https://docs.flutter.dev/. Acesso em: 12 ago. 2026.

PAGAR.ME. Documentação da API. Disponível em: https://docs.pagar.me/. Acesso em: 12 ago. 2026.

PAGAR.ME. Estorno: como estornar uma transação. Central de Ajuda Pagar.me. Disponível em: https://pagarme.helpjuice.com/pt_BR/p1-transa%C3%A7%C3%B5es-e-estornos/estorno-como-estornar-uma-transa%C3%A7%C3%A3o. Acesso em: 29 set. 2026.

RAILWAY. Pricing. Railway Documentation. Disponível em: https://docs.railway.com/pricing. Acesso em: 12 ago. 2026.

SUPABASE. Supabase documentation. Disponível em: https://supabase.com/docs. Acesso em: 29 set. 2026.

SÃO PAULO CONVENTION & VISITORS BUREAU. São Paulo registra alta de 60% no número de eventos realizados no primeiro semestre desse ano. M.I.C.E.&B. – SP para Eventos & Negócios, 8 jul. 2025. Disponível em: https://mice.visitesaopaulo.com/sao-paulo-registra-alta-de-60-no-numero-de-eventos-realizados-no-primeiro-semestre-desse-ano/. Acesso em: 12 ago. 2026.
