---
name: fechamento-financeiro
description: >-
  Month-end close and finance reconciliation from ERP data in Imans: receivables and payables,
  realized cash, settlements, cost centers and bank account balances. Use when the user asks
  "fecha o financeiro do mês", "contas a receber/a pagar", "quanto entrou no caixa?",
  "concilia recebimentos", "saldo das contas", "gastos por centro de custo", "DRE gerencial",
  "month-end close", "reconcile payments", "cash received". Do NOT use for sales performance
  (use resumo-de-vendas) or product margin (use margem-e-mix).
---

# Fechamento financeiro

Responda no idioma do usuário (padrão: português do Brasil). Os números financeiros do Imans vêm do
ERP (Olist Tiny, Bling, etc.); o Imans não ajusta nem deriva saldos.

## 1. Pré-requisitos

Esta skill precisa de `discover` e `run_query` (escopos de descoberta e consulta). Sem eles, diga que
a conexão não tem acesso ao financeiro e como liberar (reconectar o Imans marcando as permissões de
consulta). Confirme workspace e mês.

## 2. Siga a receita oficial

Leia `discover(section="workflows", key="analysis.reconcile_finance")` e siga-a. Ela define o grão de
cada entidade (`financial_entries`, `settlement_allocations`, `cash_movements`,
`cost_center_allocations`, `finance_accounts`) e as regras de sinal e de data. Resumo do que importa:

- **Competência** (obrigações) em `financial_entries`; **caixa realizado** em
  `settlement_allocations` por `paid_date`/`paid_month` e `direction`, **mais** `cash_movements`
  (lançamentos direto na conta, sem título). Nunca some os dois para a mesma coisa.
- Some os campos `signed_*` para que notas de crédito subtraiam.
- Saldo atual = `finance_accounts.latest_balance` com `latest_balance_as_of`. A série de saldos nunca
  é somada entre datas.
- Ligue os grãos por id em consultas separadas.

## 3. Roteiro do fechamento

1. Receitas e despesas do mês por competência (por categoria).
2. Caixa realizado no mês: recebido × pago, por conta.
3. Em aberto: a receber e a pagar vencidos e a vencer (próximos 30 dias).
4. Saldos das contas no fim do mês e a data de referência de cada um.
5. Centros de custo, se o usuário pedir.
6. Diferenças entre grãos (valor pago do título × alocações; gasto alocado × não alocado): **reporte,
   não corrija**. São dados do ERP.

Verifique `synced_at` antes de chamar um número de atual; se a última sincronização for antiga, avise.
Campos que tocam pedidos ou contatos exigem essas permissões: se vierem `permission_denied`, responda
sem eles e diga isso.

## 4. Resposta

Resumo executivo em 3 linhas, depois tabelas por seção acima. Rotule o que é dado do ERP e o que é
cálculo seu. Para um arquivo com todas as linhas, ofereça a skill exportar-dados.
