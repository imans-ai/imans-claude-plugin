---
name: exportar-dados
description: >-
  Exports the complete result of an Imans query (every row) as partitioned CSV or JSON files, and
  saves files Claude produced as private Imans deliverables. Use when the user asks "exporta todos
  os pedidos", "planilha completa", "baixa tudo em CSV", "salva esse relatório no Imans",
  "export all orders", "full dataset", "save this file to Imans". Do NOT use for a summary that fits
  in the chat — answer it with the analysis skills instead.
---

# Exportar dados e salvar entregáveis

Responda no idioma do usuário (padrão: português do Brasil).

## Exportar uma consulta completa (escopo query:read)

Receita oficial: `discover(section="workflows", key="analysis.export_complete_results")`.

1. Monte a consulta como em `run_query` (entidade, campos ou group_by/métricas, filtros, order_by) —
   sem `page_size` nem cursor. Confirme com o usuário entidade, período e campos antes de exportar
   dados de clientes ou pedidos em volume.
2. `start_export` com uma `idempotency_key`; um timeout pode repetir o início sem criar outra
   exportação.
3. `get_export` até `status` = `completed`. `failed` e `canceled` são finais: leia `error` e a dica.
4. Cada parte de `manifest.parts` tem link próprio de curta duração: `get_export_part_link` na hora de
   baixar (peça link novo em vez de reaproveitar). Confira `sha256` e `size_bytes`; as faixas de linhas
   somam `manifest.total_rows`.
5. Leia `consistency` antes de comparar com outras leituras (vendas usam corte `as_of`).
6. Para cancelar: `cancel_export` (pede confirmação).

## Salvar um arquivo que você gerou (escopo deliverables:write)

Receita: `discover(section="workflows", key="deliverables.publish_private_file")`.

1. Gere o arquivo você mesmo (PDF, CSV, XLSX, DOCX, PPTX, imagem, Markdown, HTML ou JSON).
2. `create_deliverable_upload` com nome, `size_bytes` exato, `sha256` e `idempotency_key`; envie os bytes
   (PUT) para a URL com os cabeçalhos retornados.
3. `finalize_deliverable`. `upload_incomplete`: termine o PUT e finalize de novo. `upload_failed`: comece
   outro upload.
4. Entregue o link do Imans; só o usuário abre. Compartilhamento público é feito por ele no Imans.

Em superfícies sem acesso a arquivos ou rede (chat), entregue os links e explique que o download é
feito pelo usuário.
