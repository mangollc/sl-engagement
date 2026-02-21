# Social Listening & Engagement

Find relevant social media conversations, draft on-brand comments, and post only after you approve each one. Works across Reddit, LinkedIn, Twitter/X, and Quora.

---

## What It Does

- **Finds conversations** where your brand can add genuine value
- **Drafts comments** that lead with helpful insight, not sales pitches
- **Never posts without your approval** — every comment requires explicit confirmation
- **Safe search** — discovery uses web search only, never touches your logged-in accounts
- **30-day window** — only surfaces recent, active conversations
- **Batched results** — shows 10 at a time with direct URLs so you can review each post

---

## Install

Open your terminal and run:

```
claude plugin marketplace add mangollc/sl-engagement
claude plugin install sl-engagement@sl-engagement
```

---

## Commands

| Command | What it does |
|---------|-------------|
| `/engage [keywords]` | Full workflow: search, draft, approve, post |
| `/monitor [keywords]` | Search and report only — no drafting or posting |
| `/search-posts [keywords]` | Quick search with batched results and direct URLs |
| `/reply-to [url]` | Draft a comment for a specific post you already found |

---

## How It Works

### 1. You provide brand context
Brand name, description, website URL, keywords, platforms, and tone preferences.

### 2. Safe discovery (read-only)
Searches using web search only — never opens Reddit, LinkedIn, Twitter, or Quora in the browser. Your logged-in sessions are untouched.

### 3. Batched results with direct URLs
Results come in batches of 10, sorted newest first. Each result shows the full post URL so you can click and review it yourself. Type "load more" for the next batch.

### 4. You pick which posts to engage with
Select posts by number (e.g., "draft for 1, 3, 7"). Only then does the tool read those specific posts.

### 5. Drafts for your approval
Each comment is drafted value-first (80% helpful content, brand mention only in the final 20%). You see the exact text before anything is posted.

### 6. Post only on your say-so
Nothing is submitted until you reply "post" or "yes". Rate-limited to avoid spam flags (2-minute gaps, max 5 per platform per session).

---

## Safety Features

- **Web search only during discovery** — never navigates to social platforms until you select specific posts
- **Never enters credentials** — if login is needed, it asks you to log in yourself
- **30-day date filter** — every search is filtered to recent posts only
- **100-result cap** — prevents runaway searches; notifies you with exact date range if capped
- **Rate limiting** — enforces gaps between posts to avoid spam detection
- **Platform compliance** — checks subreddit rules, respects character limits, follows FTC disclosure guidance
- **Engagement history** — tracks previously engaged posts to prevent duplicates across sessions

---

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) — the AI coding assistant from Anthropic
- A browser extension (Claude in Chrome) for posting comments after approval

---

## License

MIT
