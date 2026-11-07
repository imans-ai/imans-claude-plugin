# Imans for Claude

The official Imans plugin for Claude. It connects Claude to your Imans workspace (sales from ERPs and
marketplaces such as Olist Tiny, Bling, Mercado Livre, Shopee and Shopify, normalized by Imans) and
adds skills that turn common seller questions into reliable workflows.

Works in **claude.ai and Claude Desktop (chat)**, **Cowork** and **Claude Code**.

> O plugin oficial do Imans para o Claude. Conecta o Claude ao seu workspace do Imans e traz skills
> para as perguntas do dia a dia de quem vende em ERP e marketplace.

## What it includes

- **Imans MCP server** — `https://mcp.imans.ai/mcp` (remote, OAuth 2.1). You sign in on
  `app.imans.ai` and choose the workspaces, the permissions and the connection mode
  (`read_only`, `confirmation_required` or `preauthorized`).
- **Skills**

  | Skill | What it does | Reads / writes |
  |---|---|---|
  | `primeiros-passos` | Checks the connection, picks the workspace, shows what you can ask | Read |
  | `resumo-de-vendas` | Daily, weekly or monthly sales briefing | Read |
  | `diagnostico-de-vendas` | Why sales dropped or spiked, down to the orders | Read |
  | `margem-e-mix` | Realized margin by channel and product, ABC curve | Read |
  | `fechamento-financeiro` | Month-end close: receivables, payables, cash, balances (ERP data) | Read |
  | `precificacao` | Explains prices; publishes them to Olist Tiny / Bling | Read · **Write with preview + approval** |
  | `tarefas-e-processos` | Tasks, approvals, process runs and process drafts | Read · **Write with preview + approval** |
  | `exportar-dados` | Full query exports as files; saves files as private deliverables | Read · **Write** |
  | `cli` | Shell automation with the `imans` CLI (Claude Code only) | Read |

- **Commands** — `/imans:resumo`, `/imans:queda-de-vendas`, `/imans:fechamento`.

Every write tool is annotated as destructive, so Claude always asks before running it. Depending on
the connection mode, Imans also asks the user to approve the exact action inside Imans; a
`pending_approval` result is never reported as done.

## Install

**claude.ai / Claude Desktop / Cowork:** Customize → Plugins → search for **Imans** → Add. Then open
the plugin's Connectors tab and connect Imans.

**Claude Code:**

```bash
claude plugin marketplace add imans-ai/imans-claude-plugin
claude plugin install imans@imans
```

Then run `/mcp`, pick `imans` and sign in.

You need an Imans account with at least one integration synced: https://imans.ai

## Example prompts

- "Como estão as vendas deste mês comparadas ao mês passado?"
- "Por que as vendas caíram no Mercado Livre na última semana?"
- "Qual a margem por canal depois das taxas dos marketplaces? Quais produtos dão prejuízo?"
- "Fecha o financeiro de outubro: recebido, pago, em aberto e saldos."
- "Por que o SKU CAM-001 está com esse preço? Publica os preços novos no Tiny."
- "Cria uma tarefa para o Thiago revisar os pedidos devolvidos desta semana."

## Data sent

- **To Imans:** the tool arguments Claude chooses (workspace, period, filters, ids, and the content of
  tasks or drafts you ask it to create). The plugin sends an `X-Imans-Client: claude-plugin/<version>`
  header so Imans can tell plugin traffic apart. Nothing else from your conversation is sent.
- **From Imans:** only data your Imans role and this connection's consent allow. Provider credentials
  (ERP, marketplace) never leave Imans.
- **Not stored by the plugin:** the plugin is instructions and configuration only; it has no code and
  keeps no data. Imans does not read Claude's memory, chat history or files.
- Revoke access any time at https://app.imans.ai/connectors.

Privacy policy: https://imans.ai/privacy-policy · Terms: https://imans.ai/terms-of-service

## Support

support@imans.ai · Docs: https://imans.ai/docs/mcp-connectors/claude-plugin

## Development

```bash
claude plugin validate --strict .
claude --plugin-dir .              # try it locally
claude plugin eval . --trust-plugin  # eval suite with mocked Imans tools
```

## License

Apache-2.0
