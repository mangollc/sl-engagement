---
description: Draft a comment for a specific social media post URL
argument-hint: [post-url]
---

# Direct Post Engagement

Run the social-listening skill in **direct engagement** mode — user provides a specific URL, skip search.

1. User must provide a post URL. If not provided, ask for it
2. Gather brand context (brand name, description, URL, tone, disclosure preference). If already provided, extract it
3. Skip Stage 1 (search). Go directly to Stage 2: Navigate to the post and analyze context (tone, sentiment, community rules, discussion gaps)
4. Run Stage 3: Draft a comment following the skill's writing rules (value-first, brand mention in final 20%, platform-appropriate tone)
5. Run Stage 4: Present the draft for explicit user approval. Include: post URL, platform, community rules, posting account
6. Run Stage 5: Post only after user says "post", "yes", or "go ahead". Echo before submitting. Verify after posting.
7. Log to `engagement-history.md`
