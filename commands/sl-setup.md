---
description: Set up a new client brand profile for multi-client social listening
argument-hint: [brand-name]
---

# Client Brand Setup

Create a brand profile so all commands (`/sl-engage`, `/sl-monitor`, `/sl-search-posts`, `/sl-reply-to`) automatically load the right brand context without re-asking every session.

See [rules/client-setup.md](../skills/social-listening/rules/client-setup.md) for the full client config schema.

## Steps

1. Ask the user for the following (accept what they provide, don't force every field):

   **Required:**
   - **Brand name** — the name to represent in comments
   - **Brand description** — what the business does, unique value, target audience
   - **Keywords** — phrases or themes to search for (comma-separated list)

   **Optional (use defaults if not provided):**
   - **Website URL** — destination to naturally reference (default: none)
   - **Industry** — business sector (default: none)
   - **Platforms** — reddit, linkedin, twitter, quora (default: all four)
   - **Tone** — comment tone (default: "friendly, thoughtful, constructive")
   - **Disclosure** — include "I work with [brand]" in comments? (default: no)
   - **Competitors** — competitor brand names (default: none)
   - **Subreddits** — relevant subreddit names for targeted Reddit searches (default: none)

2. Generate a slug from the brand name:
   - Lowercase, replace spaces with hyphens, remove special characters
   - Check if `.sl/clients/{slug}/` already exists — if yes, append `-2`, `-3`, etc.

3. Create the directory structure:
   ```
   .sl/clients/{slug}/
   .sl/clients/{slug}/history/
   .sl/clients/{slug}/sessions/
   ```

4. Write `.sl/clients/{slug}/config.json` with all collected fields plus `created_at` set to today's date

5. Create or update `.sl/config.json`:
   - If it does not exist: create it with `default_client` set to this slug
   - If it exists but has no `default_client`: set this slug as default
   - If it already has a `default_client`: ask "Set {brand_name} as the default client? (Current default: {current_default})"

6. Add `.sl/` to the project root `.gitignore` if not already present

7. Display summary:
   ```
   Client configured: {brand_name}
   Slug: {slug}
   Keywords: {keywords list}
   Platforms: {platforms list}
   Config saved: .sl/clients/{slug}/config.json
   Default client: {yes/no}

   Ready to use:
   /sl-engage           — search, draft, and post comments
   /sl-monitor          — search and report without posting
   /sl-search-posts     — quick keyword search with direct URLs
   /sl-reply-to [url]   — draft a comment for a specific post
   ```

**This command does NOT run any search.** It only configures the brand profile.
