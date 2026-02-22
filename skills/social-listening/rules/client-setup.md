---
name: sl-client-setup
description: |
  Client profile management for multi-client social listening. Defines how to
  load, create, and switch between client brand profiles. Referenced by the
  social-listening skill and all commands.
---

# Client Setup & Management

## Directory Structure

All client data lives in `.sl/` in the project root:

```
.sl/
├── config.json                           # Global config (default_client, preferences)
└── clients/
    └── {slug}/
        ├── config.json                   # Brand context for this client
        ├── history/
        │   └── engagements.json          # All posted engagements (JSON array)
        └── sessions/
            └── session-{YYYY-MM-DD}-{N}.json  # Per-session summaries
```

`.sl/` is in `.gitignore` — client data is never committed.

---

## Global Config: `.sl/config.json`

```json
{
  "default_client": "client-slug",
  "agency_name": "",
  "preferences": {
    "default_platforms": ["reddit", "linkedin", "twitter", "quora"],
    "date_range_days": 30,
    "result_cap": 100,
    "batch_size": 10
  }
}
```

- `default_client` — the slug used when no client is specified
- `preferences` — global defaults (can be overridden per client)

---

## Client Config: `.sl/clients/{slug}/config.json`

```json
{
  "name": "Client Display Name",
  "slug": "client-slug",
  "brand_name": "Brand Name",
  "brand_description": "What the business does, unique value, target audience",
  "website_url": "https://example.com",
  "industry": "SaaS",
  "keywords": ["keyword1", "keyword2", "keyword3"],
  "platforms": ["reddit", "linkedin", "twitter", "quora"],
  "tone": "friendly, thoughtful, constructive",
  "disclosure": false,
  "competitors": ["Competitor A", "Competitor B"],
  "subreddits": ["r/relevant1", "r/relevant2"],
  "created_at": "2026-02-22",
  "notes": ""
}
```

### Required Fields
- `name` — client display name
- `slug` — auto-generated from name
- `brand_name` — the brand name to use in comments
- `brand_description` — what the business does
- `keywords` — at least 1 keyword/phrase to search for
- `platforms` — at least 1 platform

### Optional Fields
- `website_url` — defaults to empty (no link in comments)
- `industry` — for context in drafting
- `tone` — defaults to "friendly, thoughtful, constructive"
- `disclosure` — defaults to false
- `competitors` — used for filtering, not searching
- `subreddits` — targeted subreddit searches on Reddit
- `notes` — free-form notes about the client

---

## Loading Client Context

Every skill and command MUST follow this 4-step sequence:

### Step 1: Check `.sl/` exists
If `.sl/` directory does not exist, tell the user:
"No client profiles found. Run `/setup` to configure your first brand."
Stop here.

### Step 2: Client slug provided as argument
If the user passed a client slug (e.g., `/engage acme-corp`), load
`.sl/clients/{slug}/config.json`. If the slug directory does not exist,
list available clients and ask the user to choose or run `/setup`.

### Step 3: Use default client
If no slug was provided, read `.sl/config.json` and use the `default_client`
slug. Load `.sl/clients/{default_client}/config.json`.

### Step 4: List and ask
If there is no `default_client` set, list all directories under `.sl/clients/`
and present them to the user:
"Multiple client profiles found: [list]. Which one should I use?"

---

## Generating a Client Slug

Convert the brand name to a slug:
1. Lowercase all characters
2. Replace spaces with hyphens
3. Remove all characters except letters, numbers, and hyphens
4. Collapse multiple consecutive hyphens into one

Examples:
- "Joe's Plumbing Co." → `joes-plumbing-co`
- "TechStart SaaS" → `techstart-saas`
- "Acme Corp" → `acme-corp`

**Collision check:** Before creating a directory, verify `.sl/clients/{slug}/`
does not already exist. If it does, append `-2`, `-3`, etc.

---

## Engagement History: `.sl/clients/{slug}/history/engagements.json`

Format — a JSON object with an `engagements` array:

```json
{
  "engagements": [
    {
      "url": "https://reddit.com/r/smallbusiness/comments/abc123/...",
      "platform": "reddit",
      "post_title": "Best invoicing tool for freelancers?",
      "comment_posted": "That's a common pain point with invoicing...",
      "date": "2026-02-22",
      "status": "posted",
      "session_id": "2026-02-22-1"
    }
  ]
}
```

### Loading History
After loading client config, check if `engagements.json` exists. If yes, load
it and extract all `url` values into a set for de-duplication.

### De-duplication
During Stage 1 (search), compare each result URL against the history set.
Exclude any URL that already appears. If all results are duplicates, inform
the user and suggest different keywords or a broader date range.

### Saving History
After posting a comment (Stage 5), immediately append the new entry to the
`engagements` array. Write the updated file. Do NOT ask the user — always
save automatically.

### Handling Corruption
If `engagements.json` exists but fails to parse as JSON, rename it to
`engagements.json.bak`, create a fresh file, and warn the user:
"History file was corrupted — backed up to engagements.json.bak and starting
fresh."

---

## Session Summaries: `.sl/clients/{slug}/sessions/`

After each session, write a summary to:
`.sl/clients/{slug}/sessions/session-{YYYY-MM-DD}-{N}.json`

Where `N` is auto-incremented (1, 2, 3...) for multiple sessions on the same day.

```json
{
  "session_id": "2026-02-22-1",
  "client_slug": "acme-corp",
  "date": "2026-02-22",
  "search_date_range": "2026-01-23 to 2026-02-22",
  "total_results_found": 47,
  "results_reviewed": 20,
  "comments_drafted": 5,
  "comments_posted": 3,
  "comments_skipped": 2,
  "platforms_covered": ["reddit", "quora"],
  "result_cap_hit": false,
  "posted": [
    {
      "platform": "reddit",
      "title": "Best invoicing tool?",
      "url": "https://reddit.com/...",
      "status": "posted"
    }
  ],
  "skipped": [
    {
      "platform": "quora",
      "title": "How to manage invoices?",
      "url": "https://quora.com/...",
      "reason": "User skipped — too promotional"
    }
  ]
}
```

Always save automatically at session end. Do not ask.
