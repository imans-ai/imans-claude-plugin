# Imans Claude Plugin

Claude Code plugin for using the official Imans CLI with Imans workspace, catalog, and sales order data.

## What It Provides

- A Claude Code skill named `/imans:imans`.
- Guidance for installing and authenticating the `imans` CLI.
- JSON-first command patterns for agent-safe parsing and summarization.
- Safety guidance for business-sensitive workspace, catalog, and order data.

## Requirements

Install and authenticate the Imans CLI where Claude Code runs shell commands:

```bash
curl -fsSL https://imans.ai/install | bash
imans version
imans login
imans auth test --quiet
```

For automation, prefer `imans login --token-env IMANS_TOKEN` or `imans login --token-stdin < token.txt`.

## Local Testing

```bash
claude plugin validate .
claude --plugin-dir .
```

Inside Claude Code, invoke:

```text
/imans:imans
```

## Links

- Imans CLI: https://github.com/imans-ai/imans-cli
- Imans CLI releases: https://github.com/imans-ai/imans-cli/releases

## License

Apache-2.0
