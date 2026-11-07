---
name: precificacao
description: >-
  Explains Imans prices and publishes them to Olist Tiny or Bling with an exact-batch preview and
  user approval. Use when the user asks "por que esse SKU está com esse preço?", "qual o preço da
  campanha?", "o preço publicado bate com o do Imans?", "publica os preços no Tiny/Bling",
  "atualiza os preços no ERP", "why is this price", "publish prices". Do NOT use to compute
  realized margin (use margem-e-mix). Never invent or negotiate price amounts.
---

# Precificação

Responda no idioma do usuário (padrão: português do Brasil).

## Explicar um preço (somente leitura, escopo pricing:read)

1. Leia `get_pricing` sem `object` (índice). Se `setup` vier `setup_required`, repita a dica: sem
   camada base a maioria dos itens não tem preço; sem rota de publicação nada é publicado pelo Imans.
2. Explique um item com `get_pricing(object="resolved_prices", target_kind=…, target_ref=…)`.
   `layers` é a pilha que o resolvedor percorreu; exatamente uma camada está `applied`. Use
   `resolve_product` se o usuário deu nome em vez de SKU.
3. Separe sempre os quatro preços: **Preço Pretendido** (o que o Imans planeja), **preço publicado**
   (o que a rota confirmou), **preço externo observado** num desvio aberto e **margem projetada**.
   Projeção não é lucro.
4. Campanhas: camadas da Lista de Preços e sua `active_window`. Por que mudou: `price_change_logs`.

A receita oficial é `discover(section="workflows", key="pricing.explain_price")`.

## Publicar preços (escrita, escopo pricing:publish)

Siga `discover(section="workflows", key="pricing.publish_prices")`. Em resumo:

1. Leia as rotas de preço (`get_pricing`). Configuração de rota é feita no Imans, não aqui.
2. **Prévia obrigatória:** `preview_publish_prices` para a rota (opcionalmente só alguns SKUs). Os
   valores são os preços resolvidos pelo Imans — **você não escolhe valores**. Mostre ao usuário a
   lista exata (SKU, preço atual publicado, preço novo).
3. Só depois de o usuário confirmar no chat, chame `publish_prices` com **uma** `idempotency_key`
   para esse lote. O usuário aprova o lote exato no Imans em `approval.url`. Pendente não é feito.
4. Acompanhe com `get_price_publication` até terminar. Reporte por item: envio, aceite do ERP e
   verificação por releitura. Só `verified` significa que o ERP mostra o preço. `uncertain` não foi
   reenviado: peça para o usuário conferir no ERP.
5. Depois de timeout ou desconexão, **consulte o status** — nunca publique o mesmo lote com outra chave.

Conexão `read_only` não publica: diga isso e aponte onde mudar o modo da conexão no Imans.
