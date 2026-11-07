---
name: primeiros-passos
description: >-
  Onboarding to Imans in Claude: checks the Imans connection, picks the workspace and
  shows what can be asked. Use when the user says "começar com o Imans", "conectar o Imans",
  "o que o Imans faz aqui?", "qual workspace?", "get started with Imans", "connect Imans",
  "what can I ask Imans", or when an Imans tool fails with an authentication or workspace error.
  Do NOT use for an actual sales or finance question — use the matching Imans skill instead.
---

# Primeiros passos com o Imans

Responda no idioma do usuário (padrão: português do Brasil).

## Passo 0 — a conexão existe?

Procure as tools do servidor MCP `imans` (por exemplo `list_my_workspaces`, `get_sales_insights`).
Os nomes podem aparecer com prefixo do cliente (`mcp__imans__…`, `mcp__claude_ai_Imans__…`);
trate qualquer um deles como a mesma tool.

Se **nenhuma** tool do Imans estiver disponível, pare e explique como conectar:

- **claude.ai / Claude Desktop / Cowork:** Customize → Connectors → Imans → Connect. O login abre
  `app.imans.ai`, onde a pessoa escolhe os workspaces, as permissões e o modo da conexão.
- **Claude Code:** rode `/mcp`, escolha `imans` e autentique no navegador.
- Sem conta: https://imans.ai — o Imans precisa de pelo menos uma integração (ERP ou marketplace)
  sincronizada para ter dados.

Não tente responder perguntas de negócio sem a conexão e não peça tokens ou senhas no chat.

## Passo 1 — workspace

Chame `list_my_workspaces`. Com um só workspace, use-o. Com vários, mostre nome, código e papel de
cada um e pergunte qual usar (o marcado como default é a sugestão). Passe o `workspace_code` escolhido
em todas as chamadas seguintes.

## Passo 2 — o que esta conexão pode fazer

Se `discover` existir, chame-o sem `section` e leia:

- o **modo da conexão** (`read_only`, `confirmation_required`, `preauthorized`);
- quais áreas estão `available`, `permission_denied` ou `setup_required` (repita a dica literal de
  `setup_required` em vez de supor dados escondidos).

Sem `discover`, a conexão tem só o escopo básico de vendas; diga isso e siga com as tools de vendas.

## Passo 3 — mostre exemplos concretos

Ofereça de 4 a 6 perguntas que funcionam com o que está disponível, por exemplo:

- "Como estão as vendas deste mês comparadas ao mês passado?"
- "Por que as vendas caíram no Mercado Livre na última semana?"
- "Qual a margem por canal depois das taxas dos marketplaces?"
- "Fecha o financeiro de outubro: recebimentos, pagamentos e saldos."
- "Por que o SKU X está com esse preço? Publica os preços novos no Tiny."
- "Cria uma tarefa para o Thiago revisar os pedidos devolvidos."

Termine dizendo que escritas (tarefas, rascunhos de processo, publicação de preços) sempre passam por
uma prévia e, conforme o modo da conexão, por aprovação dentro do Imans.
