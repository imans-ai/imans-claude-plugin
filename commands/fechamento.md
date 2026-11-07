---
description: Fechamento do mês no Imans — vendas, margem e financeiro do ERP
argument-hint: "[mês, ex.: outubro | 2026-10] (padrão — mês anterior)"
---

Faça o fechamento do mês de $ARGUMENTS no Imans (se vazio, o mês anterior completo):

1. Vendas do mês com a skill `resumo-de-vendas` (comparação com o mês anterior e com o mesmo mês do ano passado, quando houver dados).
2. Margem por canal e produtos de atenção com a skill `margem-e-mix`.
3. Financeiro do ERP com a skill `fechamento-financeiro`, se a conexão tiver acesso; senão, diga o que falta liberar.

Entregue um relatório único: resumo executivo de 5 linhas, uma seção por item e premissas no fim.
Ofereça salvar o relatório como entregável no Imans (skill `exportar-dados`).
