---
description: Find relevant social media posts and draft on-brand comments for approval
argument-hint: [client-slug] [keywords or topic]
---

# Social Listening & Engagement

Run the full discovery-to-posting workflow from the social-listening skill.

1. Load client context from `.sl/`. See [rules/client-setup.md](../skills/social-listening/rules/client-setup.md). If a client slug is provided as the first argument, use it. Otherwise use the default client. If no `.sl/` directory exists, ask user to run `/setup` first
2. Load engagement history from `.sl/clients/{slug}/history/engagements.json` — used to exclude already-engaged posts from results
3. If additional keywords are provided as arguments (beyond the slug), add them to the client config keywords for this session only
4. Run Stage 1: Safe search using WebSearch only (never navigate to platforms). Run multiple query variations per keyword (quoted + unquoted). Cross-reference client subreddits for targeted Reddit searches. Filter to last 30 days. Cap at 100 results. De-duplicate against history. Present in batches of 10 with direct URLs
5. Wait for user to select posts, load more batches, or skip to drafting
6. Run Stage 2: Analyze context by navigating to selected posts only
7. Run Stage 3: Draft comments using tone and disclosure settings from client config
8. Run Stage 4: Present each draft for explicit user approval
9. Run Stage 5: Post only approved comments, with rate limiting (2 min gap, max 5 per platform)
10. Auto-save: append engagements to `.sl/clients/{slug}/history/engagements.json` and write session summary to `.sl/clients/{slug}/sessions/`
