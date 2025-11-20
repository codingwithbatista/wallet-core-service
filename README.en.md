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
