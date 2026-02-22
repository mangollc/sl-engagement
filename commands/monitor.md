---
description: Search social media for brand-relevant discussions without drafting comments (monitor only)
argument-hint: [client-slug] [keywords or topic]
---

# Social Media Monitor

Run the social-listening skill in **monitor only** mode — search and report, no drafting or posting.

1. Load client context from `.sl/`. See [rules/client-setup.md](../skills/social-listening/rules/client-setup.md). If a client slug is provided as the first argument, use it. Otherwise use the default client. If no `.sl/` directory exists, ask user to run `/setup` first
2. Load engagement history from `.sl/clients/{slug}/history/engagements.json` — mark previously-engaged posts in results as "[previously engaged]"
3. If additional keywords are provided as arguments, add them to the client config keywords for this session only
4. Run Stage 1: Safe search using WebSearch only (never navigate to platforms). Run multiple query variations per keyword. Cross-reference client subreddits. Filter to last 30 days. Cap at 100 results
5. Present results in batches of 10 with direct clickable URLs. Include: post title, platform, date, engagement metrics. Flag any posts already in engagement history
6. Wait for user — they can "load more" for next batch, or ask to switch to full engagement mode on specific posts
7. Auto-save session summary to `.sl/clients/{slug}/sessions/`

No comments are drafted or posted in this mode. The user reviews posts themselves by clicking the URLs.
