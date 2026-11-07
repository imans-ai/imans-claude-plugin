# Anthropic Directory submission kit

Everything to paste into https://claude.ai/directory/manage (an Owner of the Imans org submits).
Submit **two listings from the same org and pair them**:

1. **MCP connector** — `https://mcp.imans.ai/mcp`
2. **Plugin bundle** — this repository (public), pinned to the release commit

Policy 3.H: after listing, notify Anthropic of material changes to tools, descriptions, data practices
or endpoints (`mcp-review@anthropic.com`).

## Listing (both)

- **Name (≤100):** Imans
- **One-liner (≤200):** Sales, margin, finance and pricing for Brazilian sellers across ERPs and
  marketplaces — Olist Tiny, Bling, Mercado Livre, Shopee and Shopify — in one place.
- **Categories:** Analytics · Data · Finance · Sales · Productivity (pick up to 5 the portal offers)
- **Docs URL:** https://imans.ai/docs/mcp-connectors
- **Privacy policy:** https://imans.ai/privacy-policy
- **Terms:** https://imans.ai/terms-of-service
- **Support contact:** support@imans.ai
- **Icon:** `.claude-plugin/icon.png` (1024×1024). The portal keeps the icon from the first save.
- **Slug:** `imans` (permanent)

**Description (≤2000):**

> Imans connects the systems a Brazilian online seller runs on — ERPs such as Olist Tiny and Bling and
> marketplaces and stores such as Mercado Livre, Shopee and Shopify — and normalizes their orders,
> catalog, finance and pricing into one canonical model. With Imans in Claude you can ask how sales are
> going, why they dropped, which channels and products carry the margin, and close the month with the
> ERP's receivables, payables, cash and balances — all scoped to the workspaces and permissions you
> grant on the Imans consent screen.
>
> The plugin adds skills for the questions sellers ask every day: a sales briefing, a drop diagnosis
> that follows the number down to the orders behind it, margin by channel and ABC curve, month-end
> finance close, price explanation and publishing to Olist Tiny or Bling, tasks and process drafts,
> and complete data exports.
>
> Every read stays within your Imans role. Every write — creating tasks, saving process drafts,
> publishing prices — is previewed first, asks for your confirmation in Claude and, depending on the
> connection mode you chose, waits for your approval of the exact action inside Imans. Imans never
> reads Claude's memory or chat history, and ERP and marketplace credentials never leave Imans.
> Answers are in Portuguese or English.

## Use cases and prerequisites

- **Prerequisite:** an Imans account (https://imans.ai, free plan works) with at least one integration
  synced. Some areas need extra permissions on the consent screen (finance, pricing, tasks).
- Use cases: sales briefing; sales drop diagnosis; margin by channel/product; month-end finance close;
  price explanation and publishing to the ERP; tasks and approvals; process drafts; full exports.

## Example prompts (tested)

1. "Como foram as vendas de outubro comparadas a setembro?"
2. "Por que as vendas caíram no Mercado Livre na última semana?"
3. "Qual a margem por canal neste mês e quais produtos estão com margem negativa?"
4. "Fecha o financeiro de outubro: recebido, pago, em aberto e saldos das contas."
5. "Publica no Tiny os preços que mudaram no Imans." (previews the batch and asks to confirm)
6. "Cria uma tarefa para o Thiago revisar os pedidos devolvidos desta semana."

## Authentication

- OAuth 2.1 with PKCE (S256), Dynamic Client Registration at `https://mcp.imans.ai/oauth/register`.
- `client_id_metadata_document_supported: false` on purpose (see backend
  `docs/library/mcp/01_oauth_flow.md`); Claude uses DCR.
- Redirects: `https://claude.ai/api/mcp/auth_callback`; Claude Code loopback via DCR.
- Unauthenticated `/mcp` returns 401 with `WWW-Authenticate: Bearer … resource_metadata=…`;
  PRM `resource` = `https://mcp.imans.ai/mcp`; refresh tokens rotate; dead refresh → `invalid_grant`.

## Data handling

- Collected: tool arguments only (workspace, period, filters, ids, content of tasks/drafts the user
  asks for). No conversation content beyond that; nothing read from Claude's memory, history or files.
- Returned: workspace data the user's Imans role and the connection consent allow.
- Retention: per the Imans privacy policy; connection audit trail kept in Imans; user can revoke at
  https://app.imans.ai/connectors.
- Third parties: ERP/marketplace reads go through Imans; provider credentials stay in Imans.

## Tools

61 tools, every one with a `title` and `readOnlyHint: true` or `destructiveHint: true` (writes are
always destructive so Claude asks first). Reads and writes are separate tools (`preview_*` vs submit).
No tool moves money; `publish_prices` sends Imans-resolved list prices to the merchant's own ERP after
exact-batch approval. MCP Apps: `show_sales_overview_card`, `show_top_products_table` (screenshots
needed — 3–5 PNG ≥1000 px wide, cropped to the widget, prompt text supplied separately).

## Reviewer test account — TODO before submitting

- [ ] Demo workspace with realistic sample data (sales across ≥3 channels, finance, a price route in
      sandbox) and a reviewer user with all scopes; credentials go in step 8 of the portal only.
- [ ] Run every tool once as a custom connector against the demo workspace (step 8 confirmation).
- [ ] MCP Apps screenshots from the demo workspace.

## Compliance acknowledgements (step 9)

Directory guidelines · first-party API use (Imans calls its own API; provider reads are Imans'
registered integrations) · no financial transactions · no AI media generation · no prompt injection in
descriptions · no conversation data collection · public docs live.
