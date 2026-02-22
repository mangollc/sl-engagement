---
description: View, filter, or clear engagement history for a client
argument-hint: [client-slug] [view | stats | clear]
---

# Engagement History

View and manage the engagement history stored in `.sl/clients/{slug}/history/engagements.json`. See [rules/client-setup.md](../skills/social-listening/rules/client-setup.md) for the history format.

## Client Loading

Load client context following [rules/client-setup.md](../skills/social-listening/rules/client-setup.md). If a client slug is provided as the first argument, use it. Otherwise use the default client.

## Sub-commands

### `/sl-history` or `/sl-history view`

Display recent engagements for the active client.

1. Load engagement history from `.sl/clients/{slug}/history/engagements.json`
2. If no history file or empty: "No engagement history for {brand_name}. Run `/sl-engage` to get started."
3. Show most recent 20 engagements, newest first:

```
Engagement History — {brand_name}
=================================

Total: {N} engagements | Showing latest 20

#  Date        Platform   Post Title                              Status
── ──────────  ────────   ──────────                              ──────
1  2026-02-22  reddit     Best invoicing tool for freelancers?    posted
2  2026-02-21  quora      How to manage invoices efficiently?     posted
3  2026-02-20  linkedin   Invoice automation trends               posted
...

Type "show more" to see older entries.
Type "/sl-history stats" for summary statistics.
```

4. Support filtering:
   - `/sl-history view reddit` — filter by platform
   - `/sl-history view 2026-02` — filter by month
   - `/sl-history view last 7` — show last 7 days only

### `/sl-history stats`

Show engagement statistics and trends.

1. Load full engagement history
2. Calculate and display:

```
Engagement Stats — {brand_name}
================================

All time:
  Total engagements: {N}
  Platforms: reddit ({N}), linkedin ({N}), quora ({N}), twitter ({N})
  Date range: {earliest} to {latest}

Last 30 days:
  Engagements: {N}
  Most active platform: {platform} ({N})
  Avg per week: {N}

Last 7 days:
  Engagements: {N}

Sessions: {N} total
```

### `/sl-history clear`

Clear engagement history for the active client.

1. Load engagement history to get count
2. If empty: "No engagement history to clear for {brand_name}."
3. **Ask for explicit confirmation:** "Clear all {N} engagement records for {brand_name}? This cannot be undone. Previously engaged posts will show up again in future searches. Type 'clear history' to confirm."
4. Only proceed if user types the exact confirmation
5. Back up current file to `engagements.json.bak`
6. Write fresh file: `{"engagements": []}`
7. Confirm: "Engagement history cleared for {brand_name}. Backup saved to engagements.json.bak."

### `/sl-history export`

Export engagement history as a formatted summary.

1. Load full engagement history
2. Generate a markdown summary grouped by platform and date
3. Display in chat (user can copy/paste)
4. Format:

```
# Engagement Export — {brand_name}
Generated: {today}

## Reddit ({N} engagements)

### {date}
- **{post_title}**
  {url}
  Comment: "{first 100 chars of comment}..."

## LinkedIn ({N} engagements)
...
```
