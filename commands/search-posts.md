---
description: Quick search for social media posts matching keywords across platforms
argument-hint: [keywords] [platform]
---

# Quick Post Search

Fast search-only mode from the social-listening skill. No context analysis, no drafting, no posting.

1. If brand context exists in conversation, use it. Otherwise just ask for keywords and platforms
2. Run safe WebSearch queries only (never navigate to platforms). Filter to last 30 days using `after:` operator
3. Collect up to 100 results maximum. If cap hit, notify user with date range
4. Present results in batches of 10 with direct clickable URLs:
   - Post title or first line
   - Full direct URL
   - Platform, date, engagement metrics (comments, upvotes/likes)
5. User can: "load more" for next batch, or select posts to switch to full `/engage` workflow
6. Show total count and date range covered at the top of each batch
