# Wallet Core Service

**Default: Portuguese** | [Português](README.md)

## English

Welcome to **Wallet Core Service** — a backend service for personal finance tracking (transactions, accounts, categories, budgets, reporting, and import/export).

Quick links:
- Requirements: `docs/requirements.md` and `docs/requirements_functional.md` / `docs/requirements_nonfunctional.md`
- Backlog: `docs/backlog_tasks.md`

## Quick start — NestJS + TypeScript

This project will be developed using **NestJS** with **TypeScript** (Node 18+). Example quick start steps:

Prerequisites:

- Node.js 18+
- `npm` or `pnpm`/`yarn`
- PostgreSQL (local or via Docker)

Install and run (example using `npm`):

```bash
# install dependencies
npm install

# run in dev mode (Nest CLI)
npx nest start --watch

# if package.json includes scripts:
npm run start:dev

# build for production
npm run build
npm run start
```

Prisma (recommended) quick steps:

```bash
npm install prisma --save-dev
npm install @prisma/client
npx prisma init
# set DATABASE_URL in .env
npx prisma migrate dev --name init
```

`.env.example` suggested contents:

```env
DATABASE_URL=postgresql://user:password@localhost:5432/wallet_db
JWT_SECRET=replace-me-with-secure-random
```

Project layout (highlight):

- `docs/` — requirements, backlog and initial documentation.
- `.github/workflows/` — workflows including backlog bootstrap.
- `README.md` — default (Portuguese) README.

Language selection

The default README shows Portuguese. Use the link at the top or open `README.md` for the Portuguese version. GitHub does not support running client-side scripts in READMEs, so we provide both files and links.

Contributing

- Open an issue to discuss changes or improvements.
- Create a branch and open a Pull Request with a clear description.

If you want, I can scaffold a minimal NestJS project for you (with optional Prisma or TypeORM) and add a `docker-compose.yml` for Postgres — tell me which ORM you prefer or if you want no ORM for now.
# Wallet Core Service

**Default: Portuguese** | [Português](README.md)

## English

Welcome to **Wallet Core Service** — a backend service for personal finance tracking (transactions, accounts, categories, budgets, reporting, and import/export).

Quick links:
- Requirements: `docs/requirements.md` and `docs/requirements_functional.md` / `docs/requirements_nonfunctional.md`
- Backlog: `docs/backlog_tasks.md`

### Quick start (example)
1. Install dependencies and set up the environment (see `docs/` for details).
2. Configure credentials and keys (SSH/GitHub) if required.
3. Run the backend locally (e.g. FastAPI + Uvicorn):

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload
```

## Detailed instructions by stack

Choose one of the following stacks to run the backend locally.

### FastAPI (Python) - recommended for quick prototyping

Prerequisites: Python 3.10+, PostgreSQL, `pip`.

```bash
# create venv
python -m venv .venv
source .venv/bin/activate

# install example dependencies
pip install fastapi uvicorn[standard] sqlalchemy asyncpg alembic pydantic

# copy example env and edit
cp .env.example .env
# set DATABASE_URL, SECRET_KEY, etc in .env

# run migrations (example using alembic)
alembic upgrade head

# run dev server
uvicorn app.main:app --reload --port 8000
```

Notes:
- Use `DATABASE_URL=postgresql+asyncpg://user:pass@localhost:5432/wallet` in `.env`.
- Configure `alembic` or another migration tool before first deploy.
If you prefer, I can scaffold a minimal project for NestJS + TypeScript (including `package.json`, `tsconfig.json` and a `docker-compose.yml` for Postgres). Just tell me and I will create the initial files.

### Project layout (highlight)
- `docs/` — requirements, backlog and initial documentation.
- `.github/workflows/` — workflows including backlog bootstrap.
- `README.md` — default (Portuguese) README.

### Language selection
The default README shows Portuguese. Use the link at the top or open `README.md` for the Portuguese version. GitHub does not support running client-side scripts in READMEs, so we provide both files and links.

### Contributing
- Open an issue to discuss changes or improvements.
- Create a branch and open a Pull Request with a clear description.
