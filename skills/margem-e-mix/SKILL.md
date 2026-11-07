---
name: margem-e-mix
description: >-
  Profitability and product mix in Imans: realized margin overall, by channel/marketplace and by
  product, ABC curve, products with low or negative margin. Use when the user asks "qual a margem?",
  "margem por canal", "margem no Mercado Livre depois das taxas", "quais produtos dão prejuízo?",
  "curva ABC", "lucro do mês", "mix de produtos", "profit margin by channel", "ABC analysis".
  Do NOT use to explain why a price is what it is or to publish prices (use precificacao) or for
  cash/finance reconciliation (use fechamento-financeiro).
---

# Margem e mix

Responda no idioma do usuário (padrão: português do Brasil).

## Regras que não mudam

- **Margem realizada**, nunca estimada. Todo número sai de lucro e receita somados sobre pedidos e
  itens reais. Pedidos `cancelled` ou `returned` ficam fora do lucro **e** da base de receita.
- Se o custo não estiver cadastrado, o Imans informa a **cobertura de lucro** (quanto da receita tem
  custo). Com cobertura baixa (< ~80%), diga isso em destaque e não extrapole a margem para o total.
- Margem média por produto é a **média simples das margens dos produtos**; margem do canal é o lucro
  total dividido pela receita total. São números diferentes: diga qual está mostrando.
- Margem projetada (preços) não é lucro: lucro vem dos dados de vendas.

## Busca

1. `get_sales_insights` no período: KPIs, cobertura de lucro e top produtos.
2. Por canal: com `discover` + `run_query`, prefira as métricas canônicas listadas em
   `discover(section="metrics")` agrupadas por `sales_channel_name`/classificação; cite a chave da
   métrica. Sem elas, use `get_sales_by_channel` e explique o limite (sem lucro por canal).
3. Por produto: `run_query` sobre itens de pedido agrupados por produto, ou
   `get_sales_top_products` / `get_product_sales_insights` para um produto.
4. Pagine até `page.complete` antes de chamar uma lista de completa.

## Curva ABC

Ordene produtos por receita (ou lucro, se o usuário pedir), acumule a participação e classifique:
A até 80%, B até 95%, C o resto. Mostre quantos SKUs há em cada classe e a participação de cada uma.
Os cortes 80/95 são convenção: diga que são ajustáveis.

## Taxas de marketplace

Se o workspace tiver taxas, frete e comissões nos pedidos, a margem por canal já as reflete; diga
quais componentes entram (veja o descritor da entidade em `discover`). Nunca invente percentual de
comissão de marketplace.

## Resposta

1. Manchete: margem do período e o canal/produto que mais pesa.
2. Tabela por canal: receita, lucro, margem %, participação.
3. Produtos de atenção: margem negativa ou abaixo do limite que o usuário deu (padrão 10%), com
   receita para dar escala.
4. Cobertura de lucro e premissas. Marque cálculos seus como exploratórios.
5. Sugestões marcadas *(leitura)* ou *(requer ação no Imans)*; não sugira preço sem o usuário pedir.
