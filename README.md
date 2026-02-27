# n8n Reddit Automations (Templates)

Import-ready n8n workflows that turn Reddit into:

- **Leads** (buying-intent scanner + Slack/Sheets)
- **Audience research** (subreddit discovery + AI audience map)
- **Content pipeline** (Reddit threads → outlines → full blog drafts)
- **Weekly digest** (email summary + optional AI trends)

These templates are designed to be **safe to share publicly** (no credentials embedded). You paste keys/tokens inside a single config node after import.

## Templates

> Import via **n8n → Workflow → Import from URL** using the raw GitHub URL:
>
> `https://raw.githubusercontent.com/<GITHUB_OWNER>/<REPO>/main/workflows/<workflow>.json`

- **🎯 Reddit Lead Finder — Buying intent scanner**
  - Workflow: `workflows/reddit-lead-finder-buying-intent.json`
  - Setup guide: `workflows/reddit-lead-finder-buying-intent.md`

- **🔍 Subreddit Discovery — Niche audience map**
  - Workflow: `workflows/subreddit-discovery.json`
  - Setup guide: `workflows/subreddit-discovery.md`

- **🧠 Reddit Content Machine — Blog drafts from discussions**
  - Workflow: `workflows/reddit-content-machine-blog-generator.json`
  - Setup guide: `workflows/README-content-machine.md`

- **📧 Weekly Digest Email**
  - Workflow: `workflows/reddit-weekly-digest-email.json`

## How these workflows get Reddit data

All templates call an **Apify Actor** (your Reddit Scraper) via:

- `POST /v2/acts/<actorId>/run-sync-get-dataset-items?token=<APIFY_TOKEN>`
- Body is an Actor input JSON (mode: `search` / `discover` / `scrape`).

If you fork/clone the Actor, update `actorId` in the workflow config node(s).

## Repo layout

- `workflows/`: workflow JSONs + per-template setup docs
- `examples/`: sample outputs (Sheets rows, Slack message, email digest)
- `assets/`: screenshots of workflow canvases + results
- `PUBLISHING.md`: how to export/share templates

