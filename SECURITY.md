# Security policy

## How the connector protects your account

- **Read-only.** Every tool only reads data. Nothing can be changed, deleted or sent through MCP.
- **Your own data only.** A token can only see the sites, findings, flows and alerts of the account that created it.
- **Revocable tokens.** Create one token per assistant under Account → API and revoke any of them at any time.
- **No browser access.** Requests carrying a foreign `Origin` header are refused, which blocks DNS-rebinding attacks from web pages.
- **Rate limited.** The endpoint is throttled like the rest of the API.

## Keep your token safe

- Never commit a token to a repository, paste it into an issue, or share it in chat.
- Prefer clients that store secrets securely (for example, VS Code `inputs` with `"password": true`).
- If a token leaks, revoke it straight away in Account → API and create a new one.

## Reporting a vulnerability

Please email **hello@makeitflow.si** with the details and steps to reproduce. Do not open a public issue for security problems. We aim to reply within two business days.
