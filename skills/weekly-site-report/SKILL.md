---
name: weekly-site-report
description: Write a short weekly report for a business owner or client covering their websites' uptime, security, AI readiness, page changes and automation alerts, using the Make It Flow MCP connector. Use when the user asks for a weekly, monthly or client report about their sites.
---

# Weekly site report

Produce a report a non-technical owner can read in two minutes.

## Steps

1. Call `list_sites` for the list of sites and their current scores.
2. Call `get_site_report` for each site (uptime over 30 and 90 days, incidents, scores).
3. Call `get_recent_alerts` with a limit of 50 to see which automations fired, were skipped or failed.
4. Call `list_flows` to know which alerts are set up, so you can point out gaps (for instance, a site with no downtime alert).

## Report layout

1. **Headline**: one sentence on the week, such as "All three sites stayed online; one security issue needs attention."
2. **Per site**: uptime percentage, number of incidents, security score, AI-Ready score, and anything new this week.
3. **Alerts**: how many alerts were sent, and any failed deliveries that need a connection fixed.
4. **Next steps**: at most three, most important first.

## Rules

- Use only numbers the tools return. Never estimate traffic, revenue or visitor numbers.
- If a site has no data yet, say "not checked yet".
- Keep it under 300 words unless the user asks for more detail.
