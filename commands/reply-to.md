---
description: Draft a comment for a specific social media post URL
argument-hint: [client-slug] [post-url]
---

# Direct Post Engagement

Run the social-listening skill in **direct engagement** mode — user provides a specific URL, skip search.

1. User must provide a post URL. If not provided, ask for it
2. Load client context from `.sl/`. See [rules/client-setup.md](../skills/social-listening/rules/client-setup.md). If a client slug is provided as the first argument (before the URL), use it. Otherwise use the default client. If no `.sl/` directory exists, ask for brand context interactively (brand name, description, URL, tone, disclosure)
3. Check `.sl/clients/{slug}/history/engagements.json` — if this URL is already in history, warn: "This post was previously engaged on [date]. Continue anyway?"
4. Skip Stage 1 (search). Go directly to Stage 2: Navigate to the post and analyze context (tone, sentiment, community rules, discussion gaps)
5. Run Stage 3: Draft a comment using tone and disclosure settings from client config (value-first, brand mention in final 20%, platform-appropriate tone)
6. Run Stage 4: Present the draft for explicit user approval. Include: post URL, platform, community rules, posting account
7. Run Stage 5: Post only after user says "post", "yes", or "go ahead". Echo before submitting. Verify after posting
8. Auto-save: append engagement to `.sl/clients/{slug}/history/engagements.json` and write session summary to `.sl/clients/{slug}/sessions/`
