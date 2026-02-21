---
description: Find relevant social media posts and draft on-brand comments for approval
argument-hint: [keywords or topic]
---

# Social Listening & Engagement

Run the full discovery-to-posting workflow from the social-listening skill:

1. Gather brand context (brand name, description, URL, keywords, platforms, tone, disclosure preference). If already provided in conversation, extract it — don't re-ask
2. Load engagement history from `engagement-history.md` if it exists
3. Run Stage 1: Safe search using WebSearch only (never navigate to platforms). Filter to last 30 days. Cap at 100 results. Present in batches of 10 with direct URLs
4. Wait for user to select posts, load more batches, or skip to drafting
5. Run Stage 2: Analyze context by navigating to selected posts only
6. Run Stage 3: Draft comments following the skill's writing rules
7. Run Stage 4: Present each draft for explicit user approval
8. Run Stage 5: Post only approved comments, with rate limiting (2 min gap, max 5 per platform)
9. Generate session summary and offer to save to `engagement-history.md`
