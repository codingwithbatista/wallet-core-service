# Backlog de Atividades (Issues)

Este documento contém atividades (issues) detalhadas, prontas para serem criadas no GitHub como parte do backlog inicial do projeto `wallet-core-service`.

Formato: cada item abaixo pode virar uma issue com título, descrição e labels sugeridas.

-- Infra / Setup

1) Inicializar repositório com README e estrutura
Title: Inicializar repositório e README básico
Body: Criar `README.md` com descrição do projeto, instruções básicas de desenvolvimento local e links para `docs/`.
Labels: docs, infra

2) Configurar Dockerfile e .dockerignore
Title: Adicionar `Dockerfile` e `.dockerignore` para backend
Body: Dockerfile multi-stage para o backend escolhido (FastAPI/Node) e `.dockerignore` com arquivos temporários.
Labels: infra, docker

3) Criar pipeline CI básico (lint + tests)
Title: Configurar CI: lint, tests e build
Body: Adicionar workflow de CI que execute lint e testes automatizados em PRs.
Labels: ci, devops, tests

-- Banco de Dados / Migrations

4) Definir DDL inicial para PostgreSQL
Title: Especificar DDL do banco (PostgreSQL)
Body: Gerar script SQL com tabelas `users`, `accounts`, `categories`, `transactions`, `recurrings`, `budgets`, `attachments`, `audit_logs` e índices essenciais.
Labels: backend, database, sql

5) Configurar ferramenta de migrations
Title: Adicionar migrations tool e primeira migration
Body: Escolher e configurar Alembic/Flyway/Prisma Migrate/Knex e criar primeira migration baseada no DDL inicial.
Labels: backend, database, migrations

-- Backend (API)

6) Scaffold de API (FastAPI)
Title: Scaffold inicial do backend com FastAPI + Uvicorn
Body: Projeto mínimo com rotas de healthcheck, configuração de DB e estrutura de pastas (app/, tests/, alembic/).
Labels: backend, api

7) Autenticação (registro/login/JWT)
Title: Implementar autenticação com JWT
Body: Endpoints `POST /auth/register`, `POST /auth/login`, `POST /auth/refresh`, `POST /auth/forgot-password` e hashing seguro de senhas.
Labels: backend, auth, security

8) Endpoints `accounts` CRUD
Title: Implementar endpoints CRUD para `Account`
Body: `GET /accounts`, `POST /accounts`, `GET /accounts/{id}`, `PATCH /accounts/{id}`, `DELETE /accounts/{id}`. Validar ownership.
Labels: backend, api

9) Endpoints `categories` CRUD
Title: Implementar endpoints CRUD para `Category`
Body: CRUD para categorias com suporte a hierarquia (parent_id) e categorias padrão.
Labels: backend, api

10) Endpoints `transactions` CRUD
Title: Implementar endpoints CRUD para `Transaction`
Body: campos principais, validações, transferência entre contas (criar transações correlacionadas), paginação e filtros.
Labels: backend, api, transactions

11) Importação CSV/OFX
Title: Implementar importação de transações (CSV/OFX)
Body: Endpoint `POST /transactions/import` com preview, mapeamento de colunas, deduplicação e job assíncrono para processamento.
Labels: backend, import

12) Relatórios básicos (summary/monthly)
Title: Implementar endpoints de relatório
Body: `GET /reports/summary?period=monthly` e `GET /reports/category-breakdown` retornando agregações necessárias para o dashboard.
Labels: backend, reports

13) Reconciliação / marcar reconciled
Title: Endpoint para marcar transações como reconciliadas
Body: `POST /transactions/{id}/reconcile` e endpoints para reconciliar lote a partir de importação de extrato.
Labels: backend, api

14) Recorrências
Title: Implementar model e endpoints para transações recorrentes
Body: CRUD para templates de recorrência e job que gera transações futuras evitando duplicatas.
Labels: backend, scheduler

15) Orçamentos
Title: Implementar budgets endpoints
Body: CRUD para `Budget` e cálculo de progresso e alertas por categoria/periodicidade.
Labels: backend, api

-- Frontend / UI (esboço)

16) Scaffold frontend React + TypeScript
Title: Scaffold inicial do frontend (React + Vite)
Body: Projeto com autenticação básica, roteamento e layout principal.
Labels: frontend

17) Tela Dashboard (mock)
Title: Implementar tela Dashboard com dados mock
Body: Componentes para saldo consolidado, receitas vs despesas e top categorias (usar mock data inicialmente).
Labels: frontend, ui

18) Tela Lista de Transações e filtros
Title: Implementar lista de transações com filtros
Body: Lista paginada, filtros por data, conta, categoria e pesquisa por texto.
Labels: frontend, ui

19) Formulário de Transação (create/edit)
Title: Formulário para criar/editar transações
Body: Inputs para valor, data, conta, categoria, tags, anexos e opção de tornar recorrente.
Labels: frontend, ui

-- QA / Tests

20) Unit tests para lógica financeira
Title: Escrever unit tests para cálculo de saldo e regras de transferência
Body: Cobertura para regras críticas: cálculo de saldo, transferências, arredondamentos.
Labels: tests, backend

21) Integration tests para API
Title: Testes de integração (end-to-end) para principais flows
Body: Testes que cobrem: autenticação, criar conta, criar transação, gerar relatório.
Labels: tests, integration

22) E2E tests para UI (Cypress)
Title: Criar E2E tests com Cypress para fluxos críticos
Body: Testes de criação de transação, login, e geração de relatório exportável.
Labels: tests, frontend

-- Observabilidade / Segurança / Ops

23) Integrar Sentry para captura de erros
Title: Adicionar Sentry (backend) para erro e performance tracing
Body: Configurar Sentry DSN, env e integração mínima para capturar exceptions.
Labels: infra, monitoring

24) Métricas e monitoramento (Prometheus/Grafana)
Title: Exportar métricas básicas e configurar alertas
Body: Instrumentar métricas (requests, latência p90/p99, error rate) e pipeline de alertas.
Labels: infra, monitoring

25) Backup diário e política de retenção
Title: Planejar e implementar backups automáticos do Postgres
Body: Scripts ou configuração para backups diários, retenção e testes de restore.
Labels: infra, backup

-- Documentação e Processos

26) Documentar API (OpenAPI)
Title: Gerar especificação OpenAPI para endpoints principais
Body: Documentar endpoints de auth, accounts, transactions e reports; gerar Swagger UI.
Labels: docs, api

27) Criar roadmap MVP e milestones
Title: Definir milestones para MVP
Body: Criar issues/epics que agrupam tarefas para MVP e milestones no Project.
Labels: planning

28) Security review e checklist
Title: Realizar checklist de segurança
Body: Checklist para revisão de segurança: proteção de segredos, CSP, rate-limits, validação de inputs.
Labels: security, review

-- Extras (futuro)

29) Integração com agregador bancário (fase 2)
Title: Avaliar integração com agregadores (Plaid/Open Banking)
Body: Pesquisar requisitos, custos e MVP para integração com bancos.
Labels: research, integrations

30) OCR de recibos e categorização automática (ML)
Title: Prototipar OCR para anexos e categorização automática
Body: Avaliar serviços (Tesseract/Google Vision) e pipeline de ML para sugestão de categoria.
Labels: research, ml

---

Esses itens cobrem um backlog inicial abrangente. O próximo passo é criar as issues no GitHub automaticamente — forneço um workflow que faz isso e cria um Project board.
