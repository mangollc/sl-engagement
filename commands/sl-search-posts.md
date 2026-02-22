---
description: Quick search for social media posts matching keywords across platforms
argument-hint: [client-slug] [keywords] [platform]
---

# Quick Post Search

Fast search-only mode from the social-listening skill. No context analysis, no drafting, no posting.

1. Load client context from `.sl/` if it exists. See [rules/client-setup.md](../skills/social-listening/rules/client-setup.md). If a client slug is provided, use it. If `.sl/` does not exist, proceed without client context — just use the keywords and platforms provided as arguments
2. Run safe WebSearch queries only (never navigate to platforms). Use multiple query variations including quoted and unquoted keyword forms. If client config has subreddits, include targeted subreddit searches. Filter to last 30 days using `after:` operator
3. Collect up to 100 results maximum. De-duplicate against engagement history if client is loaded. If cap hit, notify user with date range
4. Present results in batches of 10 with direct clickable URLs:
   - Post title or first line
   - Full direct URL
   - Platform, date, engagement metrics (comments, upvotes/likes)
   - Flag previously-engaged posts if history is loaded
5. User can: "load more" for next batch, or select posts to switch to full `/sl-engage` workflow
6. Show total count and date range covered at the top of each batch

This command works without a client config (for quick ad-hoc searches) but benefits from one if present (subreddit targeting, de-duplication, competitor awareness).
