# EventoPedido

Pedidos digitais para eventos e shows, sem fila no caixa.

O consumidor escaneia o QR Code fixo do estabelecimento, escolhe o evento, faz o pedido pelo celular e paga via Pix ou cartão, sem instalar nada. Para retirar, apresenta no balcão o QR Code do comprovante. O operador do bar escaneia esse QR em um app nativo e confirma a entrega.

> Status: MVP em desenvolvimento. As tarefas estão no GitHub Projects **EventoPedido — MVP**, agrupadas por fase (milestones).

## Perfis

| Perfil | Acesso | Login |
|---|---|---|
| Cliente | Flutter Web, via QR Code ou link | Sem login (sessão anônima invisível) |
| Operador de bar | App Flutter nativo (Android e iOS) | Código individual de 6 caracteres, por evento |
| Estabelecimento | Painel Flutter Web | E-mail e senha (cadastro com CPF ou CNPJ) |
| Administrador | Painel Flutter Web | E-mail e senha |

## Stack

- **Backend:** Node.js 20, TypeScript 5, Express, Prisma e Zod, hospedado na Railway
- **Banco, Auth e Realtime:** Supabase (PostgreSQL 15)
- **Pagamentos:** Pagar.me v5 (Pix e cartão), com split de 5% para a plataforma
- **Apps:** Flutter (Web e nativo), com Riverpod
- **Pacotes Node:** pnpm

## Estrutura do monorepo

```
backend/          API Node.js + TypeScript + Express + Prisma
apps/web/         Flutter Web: consumidor, estabelecimento e administrador
apps/operador/    Flutter nativo (Android e iOS) do operador
packages/core/    modelos, tema e utilitários compartilhados entre os apps Flutter
docs/             especificação e protótipos
```

## Documentação

- [`docs/especificacao.md`](docs/especificacao.md): requisitos, fluxos, segurança, arquitetura, rotas da API e dicionário de dados. É a fonte da verdade.
- `docs/Documentação-Proj_01.docx`: a mesma especificação em Word
- Protótipos navegáveis (abra no navegador):
  - [`prototipo-eventobeer-azul.html`](docs/prototipos/prototipo-eventobeer-azul.html): consumidor e operador
  - [`prototipo-organizador-azul.html`](docs/prototipos/prototipo-organizador-azul.html): painel do estabelecimento
  - [`prototipo-admin-azul.html`](docs/prototipos/prototipo-admin-azul.html): painel do administrador
- [`CLAUDE.md`](CLAUDE.md): regras de arquitetura, negócio e fluxo de trabalho do projeto

## Como rodar

### Pré-requisitos

- Node.js 20 e pnpm
- Flutter (versão estável mais recente)
- Projeto no Supabase e conta sandbox no Pagar.me

### Backend

```bash
cd backend
cp .env.example .env   # preencha com os valores reais
```

Os comandos de instalação, migrations e execução entram na issue #7 (setup do backend).

### Apps Flutter

As instruções entram na issue #6 (configuração dos projetos Flutter). Os apps não usam `.env`: a configuração pública (URL da API, URL e chave anônima do Supabase, chave pública do Pagar.me) é passada com `--dart-define`.

## Fluxo de trabalho

1. Cada issue tem a sua própria branch, criada a partir da `main` atualizada: `feat/<numero>-<descricao>`, `fix/...`, `chore/...` ou `docs/...`. Não existe branch `develop`.
2. Commits em português no formato convencional: `feat:`, `fix:`, `chore:`, `refactor:`, `docs:` ou `test:`.
3. Tudo entra na `main` por pull request, com `Closes #<numero>` na descrição.
4. Antes do PR, testes, lint e checagem de tipos precisam estar passando.

## Segurança

- Nunca commitar `.env` nem credenciais. Use `backend/.env.example` como modelo.
- Dados de cartão são tokenizados no app pelo Pagar.me e nunca passam pelo backend.
- Dados bancários do estabelecimento ficam só no Pagar.me.
