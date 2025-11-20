# Requisitos Não-Funcionais - Sistema de Controle de Finanças Pessoais

Última atualização: 2025-11-20

Este documento descreve requisitos não-funcionais (RNF) — expectativas sobre segurança, desempenho, confiabilidade, escalabilidade, observabilidade, manutenção e conformidade.

1. Segurança
- RNF-S1: Criptografia em trânsito
  - TLS obrigatório (HTTPS) em todas as comunicações entre clientes e servidores.
- RNF-S2: Armazenamento e hashing
  - Senhas armazenadas com algoritmo de hashing moderno (Argon2id ou bcrypt com custo apropriado).
  - Dados sensíveis (por exemplo tokens de terceiros, informações financeiras sensíveis) devem ser criptografados em repouso quando requerido.
- RNF-S3: Controle de acesso e autorização
  - Implementar controle de acesso baseado em JWT e verificação de ownership (um usuário não pode acessar dados de outro).
- RNF-S4: Segurança de APIs
  - Limitar taxa de requisições (rate-limiting), proteção contra brute-force e abuso de endpoints de autenticação.
- RNF-S5: Proteção contra vulnerabilidades web
  - Validação e sanitização de entradas, proteção contra XSS, CSRF (quando aplicável), injeção SQL.

2. Privacidade e Conformidade
- RNF-P1: Exportação e exclusão de dados
  - Usuários podem exportar seus dados (JSON/CSV) e solicitar exclusão completa (right to erasure).
- RNF-P2: Retenção e logs
  - Logs de auditoria retidos por período configurável; dados pessoais expostos apenas no mínimo necessário.
- RNF-P3: GDPR/Lei de Proteção de Dados
  - Fornecer base para atendimento a requisitos regionais (ex.: consentimento, pedidos de exclusão).

3. Performance e Escalabilidade
- RNF-PE1: Latência
  - Operações de leitura típicas (dashboard, listagens paginadas) com latência alvo < 300ms em cargas normais.
- RNF-PE2: Throughput
  - Suportar crescimento horizontal do serviço API (stateless) e utilizar DB com replicação para leitura/escrita quando necessário.
- RNF-PE3: Caching
  - Cachear resultados pesados de relatórios ou dados agregados (Redis ou similar) com invalidação adequada.

4. Disponibilidade e Recuperação
- RNF-AV1: SLA e alta disponibilidade
  - Objetivo de disponibilidade inicial 99.9% para o serviço principal; arquitetura com redundância para componentes críticos.
- RNF-AV2: Backups e restore
  - Backups automáticos do banco (diário) com retenção configurável; procedimentos testados de restore.
- RNF-AV3: Disaster recovery
  - Plano mínimo de recuperação com RPO/RTO definidos (p.ex. RPO = 24h, RTO = 4h — ajustável para produção).

5. Observabilidade e Operações
- RNF-OBS1: Logs e correlação
  - Logs estruturados (JSON), IDs de correlação para requisições distribuídas.
- RNF-OBS2: Métricas e alertas
  - Métricas básicas (requests/sec, latência p90/p99, erro rate) expostas para Prometheus; alertas configurados no Grafana/alertmanager.
- RNF-OBS3: Tracing e erros
  - Tracing distribuído (opcional) e integração com Sentry para captura de exceções.

6. Testabilidade e Qualidade
- RNF-T1: Testes automatizados
  - Cobertura unitária para regras financeiras críticas (cálculo de saldo, geração de recorrências, importações).
  - Testes de integração para principais fluxos (autenticação, CRUD transações, geração de relatório).
- RNF-T2: Ambiente de CI/CD
  - Pipelines automáticos para build, testes e deploy; gates para evitar deploy sem testes críticos.

7. Manutenibilidade e Deploy
- RNF-M1: Separação de responsabilidades
  - Aplicação modular, API stateless, serviços independentes (quando aplicável).
- RNF-M2: Migrations e versionamento de DB
  - Uso de ferramenta de migrations (Flyway, Liquibase, Alembic, Prisma Migrate) e versionamento claro das mudanças.
- RNF-M3: Rollback e canary
  - Estratégia de deploy com capacidade de rollback e deployes canary/gradual.

8. Internacionalização e Localização
- RNF-I1: Formatos de data e moeda
  - Armazenar timestamps em UTC; permitir formatação segundo `timezone` e `locale` do usuário.
- RNF-I2: Tradução
  - Arquitetura permitindo strings traduzíveis (i18n) — iniciar com PT-BR e en-US.

9. Capacidade e Planejamento de Crescimento
- RNF-C1: Tamanho inicial
  - Suportar dezenas de milhares de usuários e centenas de milhares de transações com plano de escalonamento.
- RNF-C2: Limpeza e manutenção de dados
  - Rotinas para compactação/archiving de dados antigos se necessário.

10. Requisitos de Infraestrutura
- RNF-INF1: Armazenamento de anexos
  - Usar S3-compatible para attachments; backups e lifecycle rules (expiration) configuráveis.
- RNF-INF2: Banco de dados
  - Recomendado PostgreSQL com réplicas de leitura e backups automatizados.

11. Métricas de Sucesso (KPIs)
- Tempo médio de resposta do dashboard < 300ms.
- Disponibilidade >= 99.9% (meta inicial).
- Taxa de erros (5xx) < 0.1% no steady-state.

---

Fim do documento de requisitos não-funcionais.
