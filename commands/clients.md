---
description: List, view, switch default, or delete client brand profiles
argument-hint: [list | view <slug> | default <slug> | delete <slug>]
---

# Client Management

Manage client brand profiles stored in `.sl/clients/`. See [rules/client-setup.md](../skills/social-listening/rules/client-setup.md) for the full config schema.

## Sub-commands

### `/clients` or `/clients list`

List all configured client profiles.

1. Check `.sl/clients/` directory exists. If not: "No clients configured. Run `/setup` to create your first brand profile."
2. Read `.sl/config.json` to get `default_client`
3. List each client directory under `.sl/clients/`:

```
Client Profiles
===============

  Slug              Brand Name          Keywords                    Platforms        Default
  ────              ──────────          ────────                    ─────────        ───────
  acme-corp         Acme Corp           invoicing, billing          reddit, quora    ★
  techstart-saas    TechStart SaaS      SaaS tools, productivity    all 4
  joes-plumbing     Joe's Plumbing Co.  plumber, emergency repair   reddit

3 clients configured | Default: acme-corp
Run /clients view <slug> for full details
```

### `/clients view <slug>`

Show full config for a specific client.

1. Load `.sl/clients/{slug}/config.json`. If not found, list available slugs
2. Load engagement history count from `.sl/clients/{slug}/history/engagements.json`
3. Count session files in `.sl/clients/{slug}/sessions/`
4. Display:

```
Client: {brand_name}
Slug: {slug}
Created: {created_at}
Default: {yes/no}

Brand description: {brand_description}
Website: {website_url or "not set"}
Industry: {industry or "not set"}

Keywords: {comma-separated list}
Platforms: {comma-separated list}
Subreddits: {comma-separated list or "none"}
Tone: {tone}
Disclosure: {yes/no}
Competitors: {comma-separated list or "none"}

History: {N} engagements posted
Sessions: {N} sessions recorded
Notes: {notes or "none"}
```

### `/clients default <slug>`

Switch the default client.

1. Verify `.sl/clients/{slug}/` exists. If not, list available slugs
2. Read `.sl/config.json`
3. Update `default_client` to the new slug
4. Write `.sl/config.json`
5. Confirm: "Default client switched to {brand_name} ({slug})"

### `/clients delete <slug>`

Remove a client profile.

1. Verify `.sl/clients/{slug}/` exists. If not, list available slugs
2. Load config to get brand name
3. Count engagements and sessions that will be lost
4. **Ask for explicit confirmation:** "Delete {brand_name} ({slug})? This will remove {N} engagements and {N} session records. This cannot be undone. Type 'delete {slug}' to confirm."
5. Only proceed if user types the exact confirmation
6. Delete the `.sl/clients/{slug}/` directory and all contents
7. If this was the `default_client` in `.sl/config.json`:
   - If other clients exist, set the first one as new default and inform user
   - If no clients remain, remove `default_client` from config
8. Confirm: "Deleted client profile: {brand_name} ({slug})"
