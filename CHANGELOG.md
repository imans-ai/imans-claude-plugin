# Changelog

## 2.0.0 — 2026-11-07

- Bundles the remote Imans MCP server (`https://mcp.imans.ai/mcp`, OAuth 2.1), so the plugin works in
  claude.ai, Claude Desktop, Cowork and Claude Code.
- New skills: `primeiros-passos`, `resumo-de-vendas`, `diagnostico-de-vendas`, `margem-e-mix`,
  `fechamento-financeiro`, `precificacao`, `tarefas-e-processos`, `exportar-dados`.
- New commands: `/imans:resumo`, `/imans:queda-de-vendas`, `/imans:fechamento`.
- The CLI skill is renamed from `imans` to `cli` (`/imans:cli`) and now triggers only for shell
  automation.
- Own marketplace (`imans`) and eval suite with mocked Imans tools.

## 1.0.0 — 2026-06-05

- Initial plugin: one skill for the Imans CLI (Claude Code only).
