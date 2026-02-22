# Social Listening & Engagement

Find relevant social media conversations, draft on-brand comments, and post only after you approve each one. Works across Reddit, LinkedIn, Twitter/X, and Quora. Built for agencies managing multiple client brands.

---

## What It Does

- **Multi-client support** — manage separate brand profiles for each client
- **Finds conversations** where your brand can add genuine value
- **Drafts comments** that lead with helpful insight, not sales pitches
- **Never posts without your approval** — every comment requires explicit confirmation
- **Safe search** — discovery uses web search only, never touches your logged-in accounts
- **30-day window** — only surfaces recent, active conversations
- **Batched results** — shows 10 at a time with direct URLs so you can review each post
- **Structured history** — JSON-based tracking prevents duplicate engagements across sessions

---

## Install

Open your terminal and run:

```
claude plugin marketplace add mangollc/sl-engagement
claude plugin install sl-engagement@sl-engagement
```

---

## Getting Started

### Set Up Your First Client

```
/setup "My Brand"
```

Claude will ask for: brand description, website URL, keywords, platforms, tone, and disclosure preference. This creates a config file at `.sl/clients/my-brand/config.json` — no more re-entering brand context every session.

### Start Engaging

```
/engage                          # uses default client
/engage acme-corp                # uses specific client
/engage acme-corp "SaaS tools"   # specific client + extra keywords
/monitor                         # search and report only
/search-posts "invoicing"        # quick ad-hoc search
/reply-to https://reddit.com/... # draft for a specific post
```

---

## Commands

| Command | What it does |
|---------|-------------|
| `/setup [brand-name]` | Set up a new client brand profile |
| `/engage [client-slug] [keywords]` | Full workflow: search, draft, approve, post |
| `/monitor [client-slug] [keywords]` | Search and report only — no drafting or posting |
| `/search-posts [client-slug] [keywords]` | Quick search with batched results and direct URLs |
| `/reply-to [client-slug] [url]` | Draft a comment for a specific post you already found |

---

## Managing Multiple Clients

This plugin is built for agencies. Set up as many clients as you need:

```
/setup "Client A"
/setup "Client B"
/setup "Client C"
```

Each client gets their own directory with engagement history and session data:

```
.sl/
├── config.json                    # default client + preferences
└── clients/
    ├── client-a/
    │   ├── config.json            # brand context
    │   ├── history/
    │   │   └── engagements.json   # all posted engagements
    │   └── sessions/
    │       └── session-2026-02-22-1.json
    ├── client-b/
    │   └── ...
    └── client-c/
        └── ...
```

Use a client slug to target a specific client:

```
/engage client-a "SaaS tools"
/monitor client-b
```

If no slug is provided, the default client is used.

---

## How It Works

### 1. Brand context loads from config
Run `/setup` once per client. All commands automatically load the right brand context, keywords, platforms, and tone from `.sl/clients/{slug}/config.json`.

### 2. Safe discovery (read-only)
Searches using web search only — never opens Reddit, LinkedIn, Twitter, or Quora in the browser. Your logged-in sessions are untouched. Runs multiple query variations per keyword for better coverage.

### 3. Batched results with direct URLs
Results come in batches of 10, sorted newest first. Each result shows the full post URL so you can click and review it yourself. Type "load more" for the next batch.

### 4. You pick which posts to engage with
Select posts by number (e.g., "draft for 1, 3, 7"). Only then does the tool read those specific posts.

### 5. Drafts for your approval
Each comment is drafted value-first (80% helpful content, brand mention only in the final 20%). Uses the tone and disclosure settings from your client config. You see the exact text before anything is posted.

### 6. Post only on your say-so
Nothing is submitted until you reply "post" or "yes". Rate-limited to avoid spam flags (2-minute gaps, max 5 per platform per session).

### 7. Auto-saved history
Every posted engagement is automatically saved to structured JSON. No more duplicate posts across sessions. Session summaries track what was posted, skipped, and what to try next.

---

## Safety Features

- **Web search only during discovery** — never navigates to social platforms until you select specific posts
- **Never enters credentials** — if login is needed, it asks you to log in yourself
- **30-day date filter** — every search is filtered to recent posts only
- **100-result cap** — prevents runaway searches; notifies you with exact date range if capped
- **Rate limiting** — enforces gaps between posts to avoid spam detection
- **Platform compliance** — checks subreddit rules, respects character limits, follows FTC disclosure guidance
- **Structured engagement history** — JSON-based tracking prevents duplicate engagements across sessions, per client
- **De-duplication** — already-engaged posts are automatically excluded from search results

---

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) — the AI coding assistant from Anthropic
- A browser extension (Claude in Chrome) for posting comments after approval

---

## License

MIT
