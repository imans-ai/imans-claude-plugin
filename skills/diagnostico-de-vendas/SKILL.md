---
name: diagnostico-de-vendas
description: >-
  Root-cause analysis of a sales change in Imans: locates where a drop or spike happened (channel,
  marketplace, company, product, day) and follows it down to the orders behind it. Use when the user
  asks "por que as vendas caíram?", "o que aconteceu com o Mercado Livre/Shopee/Shopify?",
  "explica esse número", "de onde veio esse faturamento?", "esse pedido conta nas vendas?",
  "why did sales drop", "explain this revenue number". Do NOT use for a general status report
  (use resumo-de-vendas) or for margin questions (use margem-e-mix).
---

# Diagnóstico de vendas

Responda no idioma do usuário (padrão: português do Brasil). Objetivo: dizer **onde** e **por que** o
número mudou, com evidência, sem dupla contagem.

## 1. Fixe a comparação

Defina período atual e referência (mesmo tamanho, mesmos dias da semana). Se o usuário só disse
"caiu", compare os últimos 7 dias fechados com os 7 anteriores e diga isso. Confirme o workspace.

## 2. Número canônico primeiro

- Com `discover` e `run_query` disponíveis, leia a receita oficial:
  `discover(section="workflows", key="analysis.explain_sales_result")` e siga-a. Ela define os
  campos e regras do Ledger de vendas; esta skill só acrescenta o raciocínio de diagnóstico.
- Sem eles, use `get_sales_insights` e `get_sales_by_channel` para os dois períodos.

## 3. Localize a mudança (do mais largo ao mais estreito)

1. Por canal/classificação (`group_by` em `sales_channel_name` ou `order_classification_name`, ou
   `get_sales_by_channel`).
2. Dentro do canal que mais explica a variação: por empresa, depois por produto
   (`get_sales_top_products` / `get_product_sales_insights`).
3. Por dia (`get_sales_by_period`, bucket `day`) para achar o dia da quebra.

Pare quando um recorte explicar ≥70% da variação. Orçamento: ~8 chamadas antes de apresentar.

## 4. Causas a testar, em ordem

1. **Dados:** sincronização atrasada da integração (últimos dias vazios), pedidos com status ajustado
   manualmente (`status_set_manually`), reclassificação de pedidos.
2. **Devoluções e cancelamentos:** só entradas `effective` contam; devolução tardia muda o período da
   venda original. Nunca subtraia `returns_total_amount` de novo.
3. **Mix:** um ou poucos SKUs sumiram (ruptura de estoque, anúncio pausado, preço alterado).
4. **Canal:** queda concentrada em um marketplace (anúncio, reputação, frete, campanha que acabou).
5. **Calendário:** feriado, dia do pagamento, campanha no período de referência.

Para pedidos específicos, use `get_sales_order_lifecycle`: o bloco `sales_ledger` mostra a contribuição
canônica do pedido; pedido não reportável contribui zero.

Se `tunnel_read` estiver disponível e houver suspeita de atraso de sincronização, compare a leitura ao
vivo do provedor com a do Imans (somente leitura) antes de concluir.

## 5. Resposta

1. **Manchete:** quanto mudou e onde (ex.: "−R$ 42 mil (−18%), 80% no Mercado Livre, concentrado em 3 SKUs").
2. Tabela da decomposição (recorte → variação → % da variação total).
3. Causas ranqueadas, cada uma com **confiança** (alta/média/baixa) e a evidência.
4. Próximos passos, cada um marcado *(leitura)* ou *(requer ação no Imans/no canal)*.
5. Diga o que é número canônico e o que você calculou.

Não invente causa sem evidência; "não consegui determinar" é uma resposta válida.
