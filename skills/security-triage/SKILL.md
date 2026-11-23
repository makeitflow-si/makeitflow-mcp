---
name: security-triage
description: Triage open Security Watch findings for a website from the Make It Flow MCP connector and turn them into an ordered fix plan. Use when the user asks about security issues, a security score, exposed files, leaked keys, lookalike domains or outdated code on their site.
---

# Security triage

Turn a site's open security findings into a short fix plan the user can follow today.

## Steps

1. If the user did not name a site, call `list_sites` and pick the site they mean, or ask which one.
2. Call `get_security_findings` for that site.
3. Call `get_page_changes` for the same site to see whether a new third-party script or form target appeared recently, since those often explain a new finding.

## How to answer

- Group findings by severity: critical, high, medium, low. Skip low ones unless there are no others.
- For each finding give: the title, one sentence on the real-world risk, the first concrete step, and the fix guide link the tool returned.
- Treat these as urgent and say so: an exposed `.env` or config file, a leaked API key, an open admin panel, or a new script on a checkout or login page.
- When a key or password was exposed, the fix is always to revoke or rotate it with the provider, not only to delete the file.
- End with what to re-check after the fixes: the user can run a new scan from the site's Security tab in Make It Flow.

## Limits

- The connector is read-only. You cannot mark findings as fixed or accepted; tell the user to do that in their Make It Flow account.
- Findings are from passive checks of public pages and DNS. Do not present them as a full penetration test.
