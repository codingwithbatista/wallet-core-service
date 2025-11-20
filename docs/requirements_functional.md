# Requisitos Funcionais - Sistema de Controle de Finanças Pessoais

Última atualização: 2025-11-20

Sumário rápido:
- Objetivo: especificar em detalhes as funcionalidades que o sistema deve oferecer para controlar entradas e saídas financeiras, organizar categorias, orçamentos e gerar relatórios.

1. Atores
- Usuário autenticado (Usuário): pessoa que usa a aplicação para controlar suas finanças.
- Administrador: operador do sistema (aplicável em deployments multi-tenant).

2. Visão Geral de Funcionalidades Principais
- Autenticação e gestão de conta do usuário.
- Gestão de contas financeiras (contas bancárias, cartões, carteira, caixa).
- Registo e gestão de transações: receitas, despesas e transferências.
- Categorias e tags para organização de transações.
- Orçamentos por categoria e alertas.
- Transações recorrentes e geração automática de eventos futuros.
- Importação e exportação de dados (CSV/OFX/JSON).
- Relatórios e dashboards (diário, semanal, mensal, anual, por categoria).
- Reconciliação de extratos (marcar transações reconciliadas).

3. Requisitos Funcionais Detalhados

RF-001 — Autenticação e Autorização
- Descrição: permitir registro, login, logout e recuperação de senha.
- Regras:
  - Registro com e-mail e senha, validação de formato de e-mail.
  - Login retorna token de sessão (JWT) com expiração e refresh token.
  - Reset de senha via e-mail com token temporário.
  - Rotas protegidas exigem token Bearer no header Authorization.
- Critério de Aceite:
  - Usuário se registra e consegue autenticar; token válido permite acesso a endpoints protegidos.

RF-002 — Perfil do Usuário
- Descrição: permitir visualização e atualização de perfil (nome, e-mail, fuso horário, moeda padrão).
- Regras:
  - E-mail não pode ser duplicado entre usuários.
  - Mudança de moeda atualiza apenas preferência; não converte dados existentes automaticamente.
- CA:
  - Alterar fuso horário reflete em exibição de datas no dashboard do usuário.

RF-003 — Gestão de Contas Financeiras
- Descrição: CRUD de contas (`Account`) com tipos (corrente, poupança, cartão, caixa, investimento).
- Campos principais: id, user_id, name, type, currency, initial_balance, current_balance, description, metadata.
- Regras:
  - Saldo atual calculado a partir de `initial_balance` + somatório de transações confirmadas na conta.
  - Excluir conta com transações exige reatribuição de transações ou desativação; não deve remover histórico sem confirmação.
- CA:
  - Criar conta com saldo inicial e ver saldo refletido no dashboard.

RF-004 — Transações (CRUD)
- Descrição: criar, listar, atualizar e excluir transações.
- Campos principais: id, user_id, account_id, amount, currency, type (income|expense|transfer), date, category_id, tags, description, attachments, reconciled, recurring_id, created_at, updated_at.
- Regras e comportamentos:
  - `amount` deve ser positivo; `type` define o sentido (income aumenta conta, expense diminui).
  - Transferência entre contas cria duas transações ligadas: uma despesa na conta de origem e uma receita na conta de destino; a soma consolidada do usuário não é alterada.
  - Ao atualizar transação que participa de uma transferência, manter referência e consistência entre pares.
  - Exclusão exige confirmação; se transação for parte de transferência, proporcionar opção para remover ambas.
- Validações:
  - Data não pode ser futura por padrão (mas permitir com flag `future=true` para planejamento).
  - Categoria obrigatória para a maioria das despesas, dependendo de políticas configuráveis.
- CA:
  - Criar despesa com categoria X reduz o saldo da conta correspondente e aparece no relatório do mês.

RF-005 — Categorias e Subcategorias
- Descrição: CRUD para `Category` com suporte a hierarquia (parent_id), cor/ícone.
- Regras:
  - Existem categorias padrão do sistema (ex.: Alimentação, Transporte) que o usuário pode editar localmente.
  - Deletar categoria que possui transações exige reatribuição para outra categoria ou marcação como `uncategorized`.
- CA:
  - Criar categoria personalizada e atribuir a transação imediatamente disponível nos filtros.

RF-006 — Tags e Pesquisa Livre
- Descrição: permitir tags livres em transações (listas de strings); busca fulltext por descrição.
- Regras:
  - Tags são simples strings; UI sugere tags já usadas.
  - Busca textual por descrição retorna correspondências paginadas.

RF-007 — Orçamentos
- Descrição: o usuário pode criar orçamentos por categoria e período (mensal, anual).
- Campos: id, user_id, category_id, period_type (monthly|yearly), amount, start_date, end_date, alert_threshold (percent).
- Regras:
  - O sistema calcula consumo do orçamento somando despesas da categoria no período.
  - Alertas enviam notificações quando o gasto atinge `alert_threshold`% do orçamento.
- CA:
  - Criar orçamento mensal para categoria X e receber alerta ao atingir 90%.

RF-008 — Transações Recorrentes
- Descrição: modelagem de templates de transações recorrentes e geração automática.
- Campos: id, user_id, template (campos essenciais), frequency (daily, weekly, monthly, yearly), next_run_date, end_date, active.
- Regras:
  - Geração automática cria transações planejadas; UI mostra próximas execuções.
  - Permitir pausar ou cancelar recorrência; registro de execuções históricas.
  - Evitar duplicação (ex.: não gerar se já existe transação com mesma assinatura para a data).
- CA:
  - Criar recorrência mensal para aluguel que cria transação no primeiro dia do mês seguinte.

RF-009 — Importação e Exportação
- Importação CSV/OFX/JSON:
  - Endpoint/UI para enviar arquivo; pré-visualização e mapeamento de colunas (date, amount, description, category, account).
  - Detectar possíveis duplicatas (mesma data + valor + conta) e permitir mesclar/ignorar.
  - Logs de importação com relatório de erros e processadas.
- Exportação:
  - Exportar transações filtradas em CSV ou JSON.
  - Exportar relatório em CSV; exportar PDF como resumo visual.
- CA:
  - Importar um CSV de 100 linhas com preview e confirmar mapeamento.

RF-010 — Relatórios e Dashboards
- Relatórios requeridos:
  - Resumo mensal: receitas, despesas, saldo inicial/final.
  - Breakdown por categoria (valor e %).
  - Série temporal (mês a mês) para receitas/despesas.
  - Relatório anual consolidado.
  - Relatórios personalizáveis por intervalo de datas.
- Dashboard:
  - Mostra saldo consolidado por conta, receitas vs despesas do período selecionado, top 5 categorias por gasto, alertas de orçamento.
- Requisitos de visualização:
  - Gráficos interativos (hover com detalhes), tabelas paginadas e possibilidade de exportar os dados.
- CA:
  - Gerar relatório do mês atual com breakdown por categoria e exportar CSV.

RF-011 — Reconciliação de Extrato
- Descrição: marcar transações como reconciliadas e reconciliar com extrato bancário importado.
- Regras:
  - Interface para marcar transações reconciliadas; manter histórico de reconciliações.
  - Comparar extrato importado com transações existentes, sugerir correspondências (data, valor, descrição) e permitir confirmação manual.

RF-012 — Notificações e Alertas
- Descrição: avisos sobre orçamento, falhas na importação, e eventos críticos.
- Regras:
  - Configuráveis por usuário (e-mail/alerta in-app).
  - Limitar frequência de e-mails para evitar spam (rate-limiting de notificações).

RF-013 — Auditoria e Histórico
- Descrição: manter logs para ações críticas (criação/alteração/exclusão de transações, contas e usuários).
- Regras:
  - Logs armazenam: usuário, ação, entidade, timestamp e um resumo das mudanças.

RF-014 — Administração (opcional)
- Descrição: painel para administradores verem uso, número de usuários, healthy checks e logs de erro.

4. Regras de Negócio Comuns e Validações Técnicas
- Valores monetários: usar integer em centavos ou NUMERIC com precisão fixa no DB; evitar float.
- Validações de entrada: campos obrigatórios, limites de tamanho, formatos de data e moeda.
- Consistência: transações que representam transferências devem ser atômicas (tornar ambas as entradas consistentes).
- Tratamento de fusos horários: armazenar timestamps em UTC; exibir ao usuário no fuso configurado.

5. Exceções e Mensagens de Erro
- Padronizar códigos de erro HTTP e mensagens JSON com `code`, `message` e `details` opcionais.
- Exemplo: 400 para validação; 401 para auth; 403 para permissão; 404 para recurso não encontrado; 500 para erro interno.

6. Casos de Uso / User Stories (exemplos)
- US-001: Como usuário, quero registrar uma despesa rápida para que meu saldo da conta seja atualizado.
- US-002: Como usuário, quero criar um orçamento mensal por categoria para acompanhar meus gastos.
- US-003: Como usuário, quero importar meu extrato CSV e reconciliar com minhas transações existentes.

7. Critérios de Aceite (ampliados)
- CA-001: Após criar conta com saldo inicial de R$1.000, o dashboard exibe R$1.000 no saldo consolidado.
- CA-002: Criar despesa de R$100 em conta corrente reduz o saldo da conta em R$100 e aparece no relatório do mês.
- CA-003: Transferência entre conta A e B cria duas transações com mesma referência e mantém saldo consolidado.

8. Observações de Implementação
- API deve suportar paginação (limit/offset ou cursor) para endpoints de listagem.
- Endpoints de bulk (importação) devem ser assíncronos com job id para consultar status.
- Operações que alteram saldos devem ser transacionais no DB.

---

Fim do documento de requisitos funcionais.
