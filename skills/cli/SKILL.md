---
name: cli
description: >-
  Scripted Imans automation with the `imans` command-line tool in a terminal (Claude Code): shell
  scripts, CI jobs and JSON pipelines over workspace, catalog, products, variants and sales orders
  through the Imans public API. Use only when the user explicitly wants the CLI, a shell script or
  automation outside the chat (e.g. "usa o imans CLI", "script para exportar produtos", "rodar no CI").
  Do NOT use for normal questions about Imans data — the Imans MCP tools and the other Imans skills
  answer those.
---

# Imans CLI

Use `imans` to read Imans workspace, catalog, and sales order data from the official Imans public API.

## Setup

- Install: `curl -fsSL https://imans.ai/install | bash`
- Homebrew: `brew install imans-ai/tap/imans`
- Windows Scoop: `scoop bucket add imans https://github.com/imans-ai/scoop-bucket && scoop install imans`
- Verify: `imans version`
- Login interactively: `imans login`
- Login for automation: `imans login --token-env IMANS_TOKEN` or `imans login --token-stdin < token.txt`
- Test access: `imans auth test --quiet`

## Usage Rules

- Prefer `--json` for data you will parse, compare, summarize, or transform.
- Prefer `--all --json` only when the user asks for complete lists.
- Use `--profile <name>` when the user names a workspace/profile; do not switch the active profile unless asked.
- Keep responses compact; summarize data rather than pasting large JSON blocks.
- Confirm before exposing broad order/customer datasets or exporting large raw payloads.
- Never ask users to paste raw API tokens into chat.

## Commands

- Workspace metadata: `imans workspace get --json`
- Saved profiles: `imans profile list`
- Products: `imans products list --all --json`
- Product search: `imans products list --search "<query>" --json`
- Product details: `imans products get <id> --json`
- Variants: `imans product-variants list --product-id <product-id> --all --json`
- Sales orders: `imans sales-orders list --order-date-from <yyyy-mm-dd> --order-date-to <yyyy-mm-dd> --all --json`
- Sales order details: `imans sales-orders get <id> --json`
- Sales order items: `imans sales-order-items list --order-id <order-id> --all --json`
- Classifications: `imans sales-order-classifications list --all --json`

## Safety

- Treat Imans workspace, product, customer, and order data as business-sensitive.
- Avoid `imans login --token <token>` because it can leak through shell history.
- Prefer `--token-env` or `--token-stdin` for scripted auth.
- Exit code `3` means auth failed, `4` means insufficient scope, `5` means not found, `6` means network failure, and `7` means API server error.
