---
description: Search social media for brand-relevant discussions without drafting comments (monitor only)
argument-hint: [keywords or topic]
---

# Social Media Monitor

Run the social-listening skill in **monitor only** mode — search and report, no drafting or posting.

1. Gather brand context (brand name, description, keywords, platforms). If already provided, extract it
2. Load engagement history from `engagement-history.md` if it exists
3. Run Stage 1: Safe search using WebSearch only (never navigate to platforms). Filter to last 30 days. Cap at 100 results
4. Present results in batches of 10 with direct clickable URLs. Include: post title, platform, date, engagement metrics
5. Wait for user — they can "load more" for next batch, or ask to switch to full engagement mode on specific posts
6. Generate session summary listing all discovered posts, platforms, and date range
7. Offer to save the monitoring report to `engagement-history.md`

No comments are drafted or posted in this mode. The user reviews posts themselves by clicking the URLs.
