# Requisitos do Sistema de Controle de Finanças Pessoais

Última atualização: 2025-11-20

## Visão Geral
- Objetivo: Permitir que usuários controlem entradas e saídas financeiras, categorizem gastos, acompanhem orçamentos e gerem relatórios (diários, semanais, mensais e anuais).
- Escopo: Contas (banco, cartão, carteira), transações (receitas/despesas/transferências), categorias, orçamentos, recorrências, import/export, relatórios e backups.

## Personas
- Usuário Pessoal: controla finanças individuais, analisa gastos e gerencia orçamentos.
- Usuário Avançado: usa filtros, tags, importação CSV/OFX e reconciliação de extratos.
- Administrador (opcional): monitora uso e gerencia usuários em ambiente multi-tenant.

## Requisitos Funcionais (resumo)
- Autenticação e autorização: registro, login, reset de senha; suporte opcional a OAuth.
- Perfil do usuário: nome, e-mail, fuso horário, moeda padrão.
- Contas financeiras: CRUD de contas, tipos (corrente, poupança, cartão, caixa), saldo inicial e histórico.
- Transações: CRUD com campos (valor, tipo, data, conta, categoria, tags, descrição, anexos); transferências entre contas; importação CSV/OFX.
- Categorias: CRUD de categorias e subcategorias (tipo: receita/despesa), ícone/cor.
- Orçamentos: criar e acompanhar por categoria e período; alertas ao aproximar/ultrapassar.
- Transações recorrentes: criar, editar, pausar e excluir recorrências (diária, semanal, mensal, anual).
- Relatórios: dashboard, relatórios por período (diário/semanal/mensal/anual), breakdown por categoria, tendências, exportação CSV/PDF.
- Busca e filtros: por data, conta, categoria, tags, valor e texto.
- Reconciliação: marcar transações como reconciliadas para conciliar com extrato bancário.
- Import/Export e Backup: exportar histórico (CSV/JSON), importação com mapeamento, backup periódico.

## Requisitos Não-Funcionais (resumo)
- Segurança: TLS, senhas com hashing seguro (bcrypt/argon2), armazenamento cifrado de dados sensíveis (opcional).
- Performance: respostas de operações simples < 300ms para cargas modestas; paginação para listas.
- Disponibilidade: alvo 99.9% (depende da infraestrutura).
- Escalabilidade: API stateless, DB escalável, cache opcional.
- Observabilidade: logs estruturados, métricas e alertas.
- Internacionalização: suporte a localizações (iniciar PT-BR).

## Modelo de Dados (entidades principais)
- User: id, name, email, password_hash, preferred_currency, timezone, created_at, updated_at.
- Account: id, user_id, name, type, currency, initial_balance, current_balance, metadata.
- Category: id, user_id (nullable para global), name, type (income/expense), parent_id, color, icon.
- Transaction: id, user_id, account_id, amount (decimal ou integer em centavos), currency, type (income/expense/transfer), date, category_id, description, tags[], reconciled (bool), recurring_id, attachments[], created_at, updated_at.
- RecurringTransaction: id, user_id, template, interval, next_run_date, end_date, active.
- Budget: id, user_id, category_id, period, amount, alert_threshold.
- Attachment: id, transaction_id, url, filename, size, mime_type.
- AuditLog: id, user_id, action, entity_type, entity_id, timestamp, meta.

> Observação: usar decimal de precisão monetária ou integer em centavos para evitar problemas com float.

## Endpoints API Sugeridos (REST)
- Auth: `POST /auth/register`, `POST /auth/login`, `POST /auth/forgot-password`.
- Users: `GET /users/me`, `PATCH /users/me`.
- Accounts: `GET /accounts`, `POST /accounts`, `GET /accounts/{id}`, `PATCH /accounts/{id}`, `DELETE /accounts/{id}`.
- Transactions: `GET /transactions`, `POST /transactions`, `GET /transactions/{id}`, `PATCH /transactions/{id}`, `DELETE /transactions/{id}`, `POST /transactions/import`, `POST /transactions/transfer`, `POST /transactions/{id}/reconcile`.
- Categories: CRUD em `/categories`.
- Budgets: CRUD em `/budgets`.
- Recurrings: CRUD em `/recurrings`.
- Reports: `GET /reports/summary`, `GET /reports/category-breakdown`, `GET /reports/trends`, `GET /reports/export`.

Autenticação: `Authorization: Bearer <token>` (JWT) com suporte a refresh tokens.

## Relatórios e Dashboards
- Dashboard: saldo consolidado por conta, receitas vs despesas do período, top categorias, transações recentes.
- Relatórios: total receitas, despesas, saldo final por período; breakdown por categoria; séries temporais (mês a mês); projeção de cashflow baseada em recorrências.
- Exportação em CSV e PDF.

## Fluxos de UI (principais telas)
- Login / Registro / Recuperação de senha.
- Dashboard (resumo + gráficos).
- Lista de transações: pesquisa, filtros, paginação.
- Formulário de transação: criar/editar, anexar recibo, marcar recorrente.
- Contas: listagem e edição.
- Categorias: gerenciamento.
- Orçamentos: criar e ver status, alertas.
- Relatórios: gerar e exportar.
- Configurações: perfil, moeda, fuso horário, exportar/excluir dados.

## Regras de Negócio e Validações
- Valores: armazenar apenas valores absolutos; tipo define receita/despesa.
- Transferências: geram duas transações (débito/crédito) ligadas entre si.
- Exclusão de categoria com histórico: exigir reatribuição ou bloquear exclusão.
- Importação: detectar duplicatas e permitir mesclar/ignorar.
- Recorrências: evitar duplicação ao gerar eventos futuros.

## Critérios de Aceite (exemplos)
- Usuário registra-se, cria conta e vê saldo inicial no dashboard.
- Criar despesa atualiza soma do mês corretamente.
- Transferência entre contas gera duas transações e não altera saldo consolidado.
- Relatório mensal exportado em CSV contém soma por categoria correta.
- Importação CSV exibe preview de mapeamento antes de confirmar.

## Casos de Teste (alto nível)
- Unit tests: cálculo de saldo, geração de recorrências, importação e deduplicação.
- Integration tests: criar conta → criar transação → gerar relatório.
- E2E/UI: fluxo de criação de transação, filtros e exportação.
- Segurança: testes de autorização (um usuário não vê dados de outro).

## MVP (Prioridades)
1. Autenticação básica (registro/login).
2. CRUD de contas, transações e categorias.
3. Dashboard com saldo consolidado e resumo do mês.
4. Relatório mensal e exportação CSV.
5. Importação CSV básica.
6. Reconciliação manual (marcar reconciled) e export/backup.

## Roadmap de Evolução
- Fase 2: recorrências automáticas, orçamentos com alertas, anexos e tags avançadas.
- Fase 3: integração bancária (agregadores / Open Banking).
- Fase 4: ML para categorização automática e previsões de cashflow.
- Fase 5: apps mobile nativos e sincronização offline.

## Stack e Infra Sugestivos
- Backend: Node.js (Express/Nest) ou Python (FastAPI) ou Go.
- DB: PostgreSQL (usar integer em centavos ou NUMERIC de precisão monetária).
- Storage: S3-compatible para attachments.
- Frontend: React + TypeScript com biblioteca de gráficos (Chart.js/Recharts).
- Deploy: Docker, Kubernetes ou PaaS (Render, Fly.io).

## Segurança, Backups e Observabilidade
- TLS obrigatório; hashing seguro de senhas; rate-limiting.
- Backups diários do banco com testes periódicos de restore.
- Logs estruturados, métricas e alertas (Sentry/Prometheus/Grafana).

---

Se quiser, eu posso agora gerar automaticamente:
- DDL SQL para PostgreSQL baseado no modelo acima;
- Especificação OpenAPI (YAML) com endpoints principais;
- Esboço inicial de `README.md` e instruções de setup;
- Protótipo inicial em React (estrutura mínima).

Indique qual desses passos quer que eu faça a seguir.
