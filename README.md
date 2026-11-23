# Make It Flow MCP connector

Connect Claude, Cursor, VS Code or any [Model Context Protocol](https://modelcontextprotocol.io) client to your [Make It Flow](https://makeitflow.si) account, then ask in plain English:

- "Which of my sites has a critical security issue?"
- "Was any site down this week, and for how long?"
- "What changed on my checkout page since yesterday?"
- "Which of my automations failed recently?"

The connector is **read-only**. It can see your sites, scores, findings, page changes, flows and alerts. It cannot change, delete or send anything.

## What you need

1. A free Make It Flow account: <https://makeitflow.si/register>
2. At least one website added under **My sites**
3. An API token from **Account → API**: <https://makeitflow.si/account/api>

Create a separate token for each assistant, so you can revoke one without breaking the others.

## Server details

| | |
|---|---|
| Endpoint | `https://makeitflow.si/api/mcp` |
| Transport | Streamable HTTP (POST, JSON responses, no SSE stream) |
| Auth | `Authorization: Bearer <your API token>` |
| Protocol versions | `2025-06-18`, `2025-03-26`, `2024-11-05` |

## Tools

| Tool | What it returns |
|---|---|
| `list_sites` | Your monitored websites with uptime state and the latest security, AI-Ready and audit scores |
| `get_site_report` | One site: uptime over 30 and 90 days, recent incidents, security score with open issues, AI-Ready score by part, audit score |
| `get_security_findings` | Open Security Watch findings for one site, most severe first, with a link to the fix guide |
| `get_page_changes` | What Change Watch saw change on one site: new third-party scripts, form targets, noindex, titles, canonicals and big text edits |
| `list_flows` | Your automations: trigger, whether each is on, and the result of its last run |
| `get_recent_alerts` | The latest flow runs (alerts sent, skipped or failed), newest first |

## Set it up

Replace `YOUR_TOKEN` with your API token. Keep the token out of version control.

### Claude Code

```bash
claude mcp add --transport http makeitflow https://makeitflow.si/api/mcp --header "Authorization: Bearer YOUR_TOKEN"
```

### Cursor

Add to `~/.cursor/mcp.json` (see [examples/cursor.mcp.json](examples/cursor.mcp.json)):

```json
{
  "mcpServers": {
    "makeitflow": {
      "url": "https://makeitflow.si/api/mcp",
      "headers": { "Authorization": "Bearer YOUR_TOKEN" }
    }
  }
}
```

### VS Code

Add to `.vscode/mcp.json`. VS Code asks for the token once and stores it securely (see [examples/vscode.mcp.json](examples/vscode.mcp.json)):

```json
{
  "inputs": [
    { "type": "promptString", "id": "mif-token", "description": "Make It Flow API token", "password": true }
  ],
  "servers": {
    "makeitflow": {
      "type": "http",
      "url": "https://makeitflow.si/api/mcp",
      "headers": { "Authorization": "Bearer ${input:mif-token}" }
    }
  }
}
```

### Claude Desktop

Claude Desktop can reach remote servers through the [`mcp-remote`](https://www.npmjs.com/package/mcp-remote) bridge (needs Node.js). Add to `claude_desktop_config.json` (see [examples/claude_desktop_config.json](examples/claude_desktop_config.json)):

```json
{
  "mcpServers": {
    "makeitflow": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://makeitflow.si/api/mcp", "--header", "Authorization:${MIF_AUTH}"],
      "env": { "MIF_AUTH": "Bearer YOUR_TOKEN" }
    }
  }
}
```

### Test it from a terminal

```bash
curl -s https://makeitflow.si/api/mcp \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

A missing or wrong token returns HTTP 401. Requests from web browsers on other sites are refused.

## Claude skills

Ready-made [Agent Skills](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview) that use this connector. Copy a folder into your skills directory (for Claude Code, `~/.claude/skills/`) with the connector set up as above:

| Skill | What it does |
|---|---|
| [`website-health-check`](skills/website-health-check/SKILL.md) | A prioritised health summary of all your sites: what is wrong and what to fix first |
| [`security-triage`](skills/security-triage/SKILL.md) | Turns open Security Watch findings into an ordered fix plan |
| [`weekly-site-report`](skills/weekly-site-report/SKILL.md) | A two-minute weekly report for an owner or client: uptime, security, AI readiness, alerts |

## Free automation templates

Ready-to-import n8n workflows, each with a step-by-step guide. Download them free on makeitflow.si:

| Workflow | Guide and download |
|---|---|
| Missed call text-back | <https://makeitflow.si/blog/missed-call-text-back-automation> |
| Open enrollment follow-up (insurance brokers) | <https://makeitflow.si/blog/aca-open-enrollment-follow-up-automation> |
| Tax season client document chaser | <https://makeitflow.si/blog/tax-season-document-chaser-automation> |
| Order status and returns assistant (online stores) | <https://makeitflow.si/blog/order-status-returns-automation> |

More free tools: <https://makeitflow.si/tools>

## About Make It Flow

[Make It Flow](https://makeitflow.si) helps businesses of every size and industry with automation and IT. The free platform (website tools, audits, Flows automation, Security Watch, AI-Ready checks and status pages) lets small businesses solve everyday problems themselves. For bigger or industry-specific needs, the team consults and builds custom solutions. The `.si` stands for SuperIntelligence.

This repository holds the public connector documentation and examples. The Make It Flow platform itself is closed source.

## Security

See [SECURITY.md](SECURITY.md). Never paste API tokens into issues or pull requests.

## License

MIT, see [LICENSE](LICENSE).
