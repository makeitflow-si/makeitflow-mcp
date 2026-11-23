---
name: website-health-check
description: Check the health of the user's websites through the Make It Flow MCP connector (uptime, security, AI readiness, recent changes) and explain what to fix first. Use when the user asks how their sites are doing, whether anything is wrong, or what to work on next.
---

# Website health check

Use the Make It Flow MCP tools to give the user a short, prioritised health summary of their websites. The connector is read-only, so never claim to have changed or fixed anything.

## Steps

1. Call `list_sites` to see every monitored site with its uptime state and latest scores.
2. For each site that is down, has a security score under 80, an AI-Ready score under 70, or an audit score under 70, call `get_site_report` for that site.
3. For any site with open security issues in its report, call `get_security_findings`.
4. If the user mentions a recent change, a drop in traffic or something that "looks different", call `get_page_changes` for that site.

## How to answer

- Start with one line per site: name, uptime state and the most important problem, or "no issues found".
- Then list the top three actions across all sites, most urgent first. Order: site down, critical or high security findings, new third-party scripts or form targets from page changes, then everything else.
- For each security finding, give the finding title and point to its fix guide link from `get_security_findings`.
- Use plain language. Say what the problem means for the business (lost visitors, spoofed email, exposed data), not only the technical name.
- If a tool returns nothing for a site, say that the check has not run yet rather than calling it healthy.

## Limits

- Scores come from Make It Flow's own checks at the time of the last scan. Mention when the last scan was if the report includes it.
- Do not guess at causes the data does not show. Suggest the matching free tool at https://makeitflow.si/tools when the user needs a deeper check.
