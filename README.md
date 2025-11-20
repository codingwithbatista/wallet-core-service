# Wallet Core Service

**Versão padrão: Português** | [English](README.en.md)

## Português

Bem-vindo ao projeto **Wallet Core Service** — um serviço backend para controle de finanças pessoais (transações, contas, categorias, orçamentos, relatórios e importação/exportação).

Visão rápida:
- Documentação de requisitos: `docs/requirements.md` e `docs/requirements_functional.md` / `docs/requirements_nonfunctional.md`
- Backlog inicial: `docs/backlog_tasks.md`

Se você prefere ler em Inglês, clique em "English" no topo ou abra `README.en.md`.

### Como usar (rápido) — NestJS + TypeScript

Este projeto será desenvolvido usando **NestJS** com **TypeScript** (Node 18+). As instruções abaixo assumem que você está no diretório raiz do projeto.

Pré-requisitos:

- Node.js 18+ instalado
- `npm` ou `pnpm`/`yarn`
- PostgreSQL (local ou via Docker)

Instalação e execução (exemplo com `npm`):

```bash
# instalar dependências
npm install

# rodar em modo de desenvolvimento (usando Nest CLI)
npx nest start --watch

# ou, se você tiver scripts no package.json (geralmente gerados pelo Nest CLI):
npm run start:dev

# build para produção
npm run build
npm run start
```

Adicionar Nest CLI globalmente (opcional):

```bash
npm i -g @nestjs/cli
# criar novo projeto com CLI (se preferir scaffold automático)
npx @nestjs/cli new backend --package-manager npm
```

Banco de dados e ORM (opcional):

- Recomendado: **Prisma** (moderno, tipado). Exemplo de passos:

```bash
npm install prisma --save-dev
npm install @prisma/client
npx prisma init
# configurar DATABASE_URL em .env
npx prisma migrate dev --name init
```

- Alternativa: TypeORM (integrado ao Nest com `@nestjs/typeorm`).

Arquivo de exemplo `.env` (sugestão):

```env
DATABASE_URL=postgresql://user:password@localhost:5432/wallet_db
JWT_SECRET=replace-me-with-secure-random
```

Observação sobre idiomas

Este README mostra por padrão a versão em Português. Para ler em Inglês use o link no topo (`README.en.md`).

Scaffold

Se quiser, posso gerar um scaffold inicial com NestJS + TypeScript, `package.json`, `tsconfig.json`, um módulo `app` básico, e um `docker-compose.yml` com Postgres — diga se prefere que eu crie também suporte a Prisma ou TypeORM.

### Estrutura relevante
- `docs/` — requisitos, backlog e documentação inicial.
- `.github/workflows/` — workflow para bootstrap do backlog.
- `README.en.md` — versão em Inglês.

### Seleção de idioma
Por padrão o README apresenta a versão em Português. Para ver a versão em Inglês use o link no topo (ou abra `README.en.md`). No GitHub não é possível executar script client-side no README; por isso oferecemos os dois arquivos e links para a versão desejada.

### Contribuindo
- Abra uma issue para discutir mudanças ou sugerir melhorias.
- Faça um fork/branch e envie Pull Requests com descrições claras.

---

Se quiser, posso também:
- atualizar este README com instruções específicas para o stack (FastAPI ou Node), ou
- adicionar badges, templates de PR/Issue e seção de roadmap visual.
