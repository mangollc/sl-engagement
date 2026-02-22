# Social Listening & Engagement

A Claude Code plugin that finds relevant social media conversations across Reddit, LinkedIn, Twitter/X, and Quora, drafts on-brand comments, and posts them only after your explicit approval. Built for agencies managing multiple client brands.

**Completely free.** No API keys, no subscriptions, no external services. Runs entirely on your local machine using Claude Code + web search.

---

## Table of Contents

- [What It Does](#what-it-does)
- [Requirements](#requirements)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Commands Reference](#commands-reference)
- [How It Works](#how-it-works)
- [Multi-Client Management](#multi-client-management)
- [Configuration](#configuration)
- [Use Cases](#use-cases)
- [Safety & Compliance](#safety--compliance)
- [Architecture](#architecture)
- [FAQ](#faq)
- [License](#license)

---

## What It Does

- **Finds conversations** where your brand can add genuine value across 4 platforms
- **Drafts comments** that lead with helpful insight (80% value, 20% brand mention)
- **Never posts without your approval** — every comment requires explicit "yes" or "post"
- **Multi-client support** — manage separate brand profiles for each client
- **Safe search** — discovery uses web search only, never touches your logged-in social accounts
- **Structured history** — JSON-based tracking prevents duplicate engagements across sessions
- **Smart de-duplication** — excludes previously-engaged posts and removes duplicate URLs within searches
- **Configurable** — date range, result cap, batch size, tone, disclosure, and more

---

## Requirements

| Requirement | Purpose |
|------------|---------|
| [Claude Code](https://docs.anthropic.com/en/docs/claude-code) | The AI coding assistant that runs the plugin |
| [Claude in Chrome](https://chromewebstore.google.com/) | Browser extension for posting comments after your approval |
| A browser with active logins | You need to be logged into Reddit, LinkedIn, Twitter/X, or Quora in your browser for posting |

**No API keys or paid services required.** All discovery is done via web search.

---

## Installation

### Step 1: Add the marketplace

```bash
claude plugin marketplace add mangollc/sl-engagement
```

### Step 2: Install the plugin

```bash
claude plugin install sl-engagement@sl-engagement
```

### Step 3: Verify installation

```bash
claude plugin list
```

You should see `sl-engagement@sl-engagement` with status `enabled`.

### Updating

To get the latest version:

```bash
claude plugin marketplace update mangollc/sl-engagement
claude plugin install sl-engagement@sl-engagement
```

---

## Quick Start

### 1. Set up your first client brand

```
/sl-setup "My Brand Name"
```

Claude will ask you for:
- **Brand description** — what the business does, who it serves
- **Keywords** — phrases to search for (e.g., "invoicing software", "freelance billing")
- **Platforms** — which platforms to search (reddit, linkedin, twitter, quora)
- **Website URL** — to naturally reference in comments (optional)
- **Tone** — comment style (default: friendly, thoughtful, constructive)
- **Disclosure** — whether to disclose brand affiliation (default: no)
- **Subreddits** — specific subreddit names for targeted Reddit searches (optional)
- **Competitors** — competitor names for filtering (optional)

This creates a persistent config at `.sl/clients/my-brand-name/config.json`. You only do this once per client.

### 2. Find and engage with relevant posts

```
/sl-engage
```

This runs the full workflow:
1. Searches across all configured platforms using your keywords
2. Presents results in batches of 10 with direct URLs
3. You click the URLs to review posts yourself
4. You tell Claude which posts to draft comments for (e.g., "draft for 1, 3, 7")
5. Claude drafts value-first comments using your brand's tone
6. You review each draft and say "post" or "skip"
7. Claude posts the approved comments through your browser
8. Everything is saved to engagement history automatically

### 3. Check what you've done

```
/sl-history stats
```

---

## Commands Reference

All commands are prefixed with `sl-` to avoid conflicts with other plugins.

### `/sl-setup [brand-name]`

Create a new client brand profile.

```
/sl-setup "Acme Corp"
/sl-setup "Joe's Plumbing"
/sl-setup "TechStart SaaS"
```

- Collects brand details interactively (only brand name, description, and keywords are required)
- Generates a URL-safe slug from the brand name
- Creates the directory structure: `.sl/clients/{slug}/`
- Sets as default client if it's the first one
- Adds `.sl/` to `.gitignore`
- Does NOT run any search — config only

---

### `/sl-engage [client-slug] [keywords]`

Full engagement workflow: search, draft, approve, and post.

```
/sl-engage                              # uses default client
/sl-engage acme-corp                    # uses specific client
/sl-engage acme-corp "SaaS tools"       # specific client + extra keywords
/sl-engage "freelance invoicing"        # default client + extra keywords
```

**Workflow:**
1. Safe search across configured platforms (web search only — no browser)
2. Results presented in batches with direct clickable URLs
3. You select which posts to draft comments for
4. Claude reads the selected posts and drafts value-first comments
5. Each draft presented for your explicit approval
6. Posts only after you say "yes" or "post"
7. Rate limited: 2-minute gap between posts, max 5 per platform per session
8. History auto-saved after each post

---

### `/sl-monitor [client-slug] [keywords]`

Search and report only — no drafting, no posting.

```
/sl-monitor                             # monitor default client's keywords
/sl-monitor acme-corp                   # monitor specific client
/sl-monitor "AI tools"                  # monitor with extra keywords
```

- Same search as `/sl-engage` but stops at results presentation
- Flags posts you've previously engaged with as "[previously engaged]"
- Great for daily check-ins to see what's being discussed
- You can click URLs to read posts yourself
- Can switch to full engagement mode on specific posts if you choose

---

### `/sl-search-posts [client-slug] [keywords] [platform]`

Quick ad-hoc search with no commitment.

```
/sl-search-posts "invoicing"                  # search across all platforms
/sl-search-posts "invoicing" reddit            # search Reddit only
/sl-search-posts acme-corp "billing software"  # use client config + extra keywords
```

- Works without a client config (for quick one-off searches)
- Benefits from client config if present (subreddit targeting, de-duplication)
- Results in batches with direct URLs
- Can switch to full `/sl-engage` workflow on selected posts

---

### `/sl-reply-to [client-slug] [url]`

Draft a comment for a specific post you've already found.

```
/sl-reply-to https://reddit.com/r/freelance/comments/abc123/...
/sl-reply-to acme-corp https://www.quora.com/How-do-I-manage-invoices
```

- Skips the search step entirely
- Warns if you've already engaged with this URL before
- Reads the post, analyzes context and community rules
- Drafts a comment using your brand's tone and disclosure settings
- Presents for your approval before posting

---

### `/sl-clients [sub-command]`

Manage your client brand profiles.

```
/sl-clients                    # list all clients
/sl-clients list               # same as above
/sl-clients view acme-corp     # show full config for a client
/sl-clients default acme-corp  # switch the default client
/sl-clients delete acme-corp   # remove a client (requires confirmation)
```

**Sub-commands:**

| Sub-command | What it does |
|-------------|-------------|
| `list` (default) | Show all client profiles with keywords, platforms, and default status |
| `view <slug>` | Show full config, engagement count, and session count for a client |
| `default <slug>` | Switch which client is used when no slug is specified |
| `delete <slug>` | Remove a client profile and all its data (requires typing confirmation) |

---

### `/sl-history [client-slug] [sub-command]`

View and manage engagement history.

```
/sl-history                         # show recent engagements for default client
/sl-history view                    # same as above
/sl-history acme-corp               # show history for specific client
/sl-history view reddit             # filter by platform
/sl-history view 2026-02            # filter by month
/sl-history view last 7             # show last 7 days
/sl-history stats                   # show engagement statistics
/sl-history clear                   # clear history (requires confirmation)
/sl-history export                  # export as formatted markdown
```

**Sub-commands:**

| Sub-command | What it does |
|-------------|-------------|
| `view` (default) | Show recent engagements with filtering options |
| `stats` | Show totals, platform breakdown, weekly averages |
| `clear` | Clear all history for a client (backs up to `.bak` first) |
| `export` | Export history as formatted markdown grouped by platform and date |

---

## How It Works

### The 5-Stage Workflow

```
Stage 1: Search & Discover (SAFE — web search only, no browser)
    ↓
Stage 2: Analyze Context (browser opens ONLY for posts you selected)
    ↓
Stage 3: Draft Comment (value-first, brand mention in final 20%)
    ↓
Stage 4: Present for Approval (you see exact text before anything is posted)
    ↓
Stage 5: Post After Approval (only after you say "yes" or "post")
```

### Search Strategy

For each keyword in your client config, Claude runs **multiple query variations** to maximize coverage:

- **Quoted** (exact match): `site:reddit.com "invoicing software" after:2026-01-23`
- **Unquoted** (broad match): `site:reddit.com invoicing software after:2026-01-23`
- **Subreddit-targeted**: `site:reddit.com/r/freelance "invoicing" after:2026-01-23`
- **Advice variants**: `site:reddit.com "invoicing recommendation" OR "invoicing advice" after:2026-01-23`

All searches use `site:` operators and `after:` date filters. The tool searches both `twitter.com` and `x.com` for Twitter/X coverage.

### Comment Drafting Rules

- First 80% of the comment must stand alone as genuinely helpful — no brand mention
- Brand mentioned only in the final 20%, maximum once, and only if it fits naturally
- If disclosure is enabled: "Full disclosure: I'm part of the [brand] team"
- Platform-appropriate tone and formatting (Reddit markdown, Twitter 280 chars, etc.)
- No marketing language: no "leverage", "game-changer", "seamless", "solutions"
- No generic openers: no "Great question!" or "This is a really interesting topic"

---

## Multi-Client Management

This plugin is designed for agencies managing multiple brands. Each client gets isolated config, history, and sessions.

### Setting Up Multiple Clients

```
/sl-setup "Client A"
/sl-setup "Client B"
/sl-setup "Client C"
```

### Directory Structure

```
.sl/
├── config.json                    # default client + global preferences
└── clients/
    ├── client-a/
    │   ├── config.json            # brand context, keywords, tone
    │   ├── history/
    │   │   └── engagements.json   # all posted engagements (JSON)
    │   └── sessions/
    │       └── session-2026-02-22-1.json
    ├── client-b/
    │   └── ...
    └── client-c/
        └── ...
```

### Targeting a Specific Client

Pass the client slug as the first argument to any command:

```
/sl-engage client-a "SaaS tools"
/sl-monitor client-b
/sl-history client-c stats
/sl-clients view client-a
```

If no slug is provided, the default client is used. Switch the default with:

```
/sl-clients default client-b
```

---

## Configuration

### Global Config: `.sl/config.json`

```json
{
  "default_client": "acme-corp",
  "preferences": {
    "default_platforms": ["reddit", "linkedin", "twitter", "quora"],
    "date_range_days": 30,
    "result_cap": 100,
    "batch_size": 10
  }
}
```

| Setting | Default | What it controls |
|---------|---------|-----------------|
| `default_client` | First client created | Which client is used when no slug is specified |
| `date_range_days` | 30 | How far back to search (in days) |
| `result_cap` | 100 | Maximum results per search session |
| `batch_size` | 10 | How many results to show per batch |
| `default_platforms` | All 4 | Platforms to search when not specified per client |

Edit this file directly to change global preferences.

### Client Config: `.sl/clients/{slug}/config.json`

```json
{
  "name": "Acme Corp",
  "slug": "acme-corp",
  "brand_name": "Acme",
  "brand_description": "Invoicing and billing software for freelancers",
  "website_url": "https://acme.com",
  "industry": "SaaS",
  "keywords": ["invoicing software", "freelance billing", "invoice automation"],
  "platforms": ["reddit", "linkedin", "quora"],
  "tone": "friendly, thoughtful, constructive",
  "disclosure": false,
  "competitors": ["FreshBooks", "Wave"],
  "subreddits": ["freelance", "smallbusiness", "Entrepreneur"],
  "created_at": "2026-02-22",
  "notes": ""
}
```

| Field | Required | Description |
|-------|----------|-------------|
| `brand_name` | Yes | The name used in comments |
| `brand_description` | Yes | What the business does — used for drafting context |
| `keywords` | Yes | Search phrases (at least 1) |
| `platforms` | Yes | Which platforms to search (at least 1) |
| `website_url` | No | URL to reference in comments |
| `industry` | No | Business sector for drafting context |
| `tone` | No | Comment style (default: friendly, thoughtful, constructive) |
| `disclosure` | No | Include "I work with [brand]" (default: false) |
| `competitors` | No | Competitor names for filtering |
| `subreddits` | No | Subreddit names WITHOUT `r/` prefix for targeted searches |
| `notes` | No | Free-form notes about the client |

---

## Use Cases

### 1. Daily Brand Monitoring

Check what people are saying about your brand or industry every morning:

```
/sl-monitor
```

Review the results, click interesting URLs, and decide if any need a response. If yes, tell Claude to draft comments for specific posts.

### 2. Weekly Engagement Sessions

Run a full engagement session once or twice a week:

```
/sl-engage
```

Search, review, draft, approve, and post — all in one flow. History tracking ensures you never comment on the same post twice.

### 3. Responding to a Specific Post

Found a great post on Reddit while browsing? Draft a comment for it:

```
/sl-reply-to https://reddit.com/r/smallbusiness/comments/abc123/best-invoicing-tool
```

### 4. Agency Managing Multiple Brands

Set up each client with their own brand context:

```
/sl-setup "Acme Invoicing"
/sl-setup "TechStart SaaS"
/sl-setup "GreenClean Products"
```

Run engagement sessions for each client:

```
/sl-engage acme-invoicing
/sl-engage techstart-saas
/sl-engage greenclean-products
```

Track each client's history separately:

```
/sl-history acme-invoicing stats
/sl-history techstart-saas stats
```

### 5. Exploring New Markets

Search for conversations in a new niche before committing:

```
/sl-search-posts "sustainable cleaning products" reddit
```

No client config needed — works as a quick ad-hoc search.

### 6. Competitor Intelligence

Set up competitors in your client config, then monitor what people say:

```
/sl-monitor acme-invoicing
```

Posts comparing your brand vs competitors are flagged as potential high-value engagement opportunities.

### 7. Product Launch Engagement

Add product-specific keywords for a launch period:

```
/sl-engage acme-invoicing "new invoice templates"
```

Extra keywords are used for this session only — your saved config stays unchanged.

### 8. Platform-Specific Campaigns

Focus engagement on one platform at a time:

```
/sl-search-posts acme-invoicing "invoicing" reddit
/sl-search-posts acme-invoicing "invoicing" quora
```

### 9. Reviewing Engagement Performance

Check how your engagement is performing:

```
/sl-history stats              # overall stats
/sl-history view reddit        # filter by platform
/sl-history view last 7        # last week's activity
/sl-history export             # full export for reporting
```

---

## Safety & Compliance

### Search Safety

- **Web search only during discovery** — Claude never opens Reddit, LinkedIn, Twitter, or Quora in your browser during search (Stage 1)
- **Your logged-in accounts are never touched** until you explicitly select posts to engage with
- **No API keys or credentials needed** — all discovery is done through web search

### Posting Safety

- **Never enters your passwords** — if login is required, Claude asks you to log in yourself
- **Every comment requires explicit approval** — you must say "yes", "post", or "go ahead"
- **Exact text shown before posting** — what you see is what gets posted, no modifications
- **Rate limiting** — minimum 2 minutes between posts, maximum 5 per platform per session

### Platform Compliance

- **Reddit:** Respects 10:1 self-promotion ratio, checks subreddit rules, max 2-3 comments per subreddit per session
- **LinkedIn:** Avoids high-volume templated commenting, no competitor post engagement
- **Twitter/X:** Verifies 280-character limit, spaces out replies, never replies to same user repeatedly
- **Quora:** Answers must directly address questions, checks Space rules

### FTC Compliance

If a material connection exists (employee, contractor, paid relationship), the `disclosure` setting in client config enables automatic disclosure in comments (e.g., "Full disclosure: I'm part of the [brand] team").

### Data Privacy

- All client data stored locally in `.sl/` directory
- `.sl/` is added to `.gitignore` — never committed to version control
- No data sent to external services beyond web search queries
- Engagement history stays on your machine

---

## Architecture

### How It Runs

This is a **Claude Code plugin** — it runs entirely on your local machine inside Claude Code. There is:

- No cloud server or background process
- No scheduled jobs or cron tasks
- No webhook listeners or real-time alerts
- No database or backend service

Every action happens **on-demand when you type a command**. Claude Code reads your config, runs web searches, opens your browser (only when you select posts), and writes history files locally.

### Technology Stack

| Component | Technology |
|-----------|-----------|
| Search engine | Google Web Search via Claude Code's WebSearch tool |
| Browser automation | Claude in Chrome extension |
| Data storage | Local JSON files in `.sl/` directory |
| Configuration | JSON config files per client |
| Runtime | Claude Code (Anthropic) |

### Platform Support

| Platform | Search method | Post method |
|----------|--------------|-------------|
| Reddit | `site:reddit.com` + `site:reddit.com/r/{subreddit}` | Browser (Claude in Chrome) |
| LinkedIn | `site:linkedin.com/posts` + `site:linkedin.com/pulse` | Browser (Claude in Chrome) |
| Twitter/X | `site:twitter.com` + `site:x.com` | Browser (Claude in Chrome) |
| Quora | `site:quora.com` | Browser (Claude in Chrome) |

---

## FAQ

**Q: Does this run in the background?**
No. It only runs when you type a command. There's no background monitoring or scheduled searches.

**Q: Do I need API keys for Reddit, Twitter, etc.?**
No. All discovery uses web search. You only need to be logged into those platforms in your browser for posting.

**Q: Will this affect my logged-in social media accounts?**
Not during search. Your browser is only used when you select specific posts to engage with (Stage 2+). The search phase (Stage 1) uses web search exclusively.

**Q: Can I use this without setting up a client?**
Yes — `/sl-search-posts "your keyword"` works without any client config for quick ad-hoc searches. For drafting and posting, you'll want a client config so Claude knows your brand's tone and context.

**Q: How does de-duplication work?**
Two levels: (1) Your engagement history prevents re-engaging with posts you've already commented on. (2) URL normalization removes duplicate results within the same search session (e.g., `www.reddit.com` vs `old.reddit.com` for the same post).

**Q: What happens if I have multiple plugins with similar commands?**
All SL commands are prefixed with `sl-` (e.g., `/sl-setup`, `/sl-engage`) to avoid conflicts with other plugins.

**Q: Can multiple people use this for the same client?**
The `.sl/` directory is local to each machine. If multiple people work on the same client, each person has their own engagement history. They won't see each other's posted comments in the de-duplication check.

**Q: What platforms are supported?**
Reddit, LinkedIn, Twitter/X, and Quora. The architecture supports adding more platforms by extending the search queries and platform compliance rules.

---

## License

MIT
