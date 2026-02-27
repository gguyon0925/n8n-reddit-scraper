## n8n workflow templates (how to publish)

This folder is for **exported n8n workflow JSON** files that use this Apify Actor.

### Quick import (recommended)

In n8n:

1. Open the workflow canvas
2. Click the menu (three dots) → **Import from File** (or **Import from URL**)
3. Import one of the JSON files from `n8n/workflows/`
4. Open the **Config (edit me)** node and replace placeholder values:
   - `APIFY_TOKEN_HERE`
   - `spry_wholemeal~reddit-scraper` (default Actor ID; if you fork/clone the Actor, replace it)
   - destination settings (Slack webhook / Google Sheet ID / Notion DB ID / email)

These templates intentionally avoid embedding credentials inside nodes, so they are safe to publish publicly.

### Recommended structure

- `n8n/workflows/`
  - `reddit-domain-to-slack.json`
  - `reddit-search-to-slack.json`
  - `reddit-discover-to-sheets.json`
  - `reddit-scrape-ai-to-notion.json`
  - `reddit-weekly-digest-email.json`

### Create a template

1. Run n8n locally (free): `npx n8n`
2. Install the Apify community node: Settings → Community Nodes → `@apify/n8n-nodes-apify`
3. Build your workflow (typically: Trigger → Apify “Run Actor” → Filter/Format → Destination)
4. Export the workflow JSON and save it under `n8n/workflows/`

### Share as “Import from URL”

After pushing to GitHub, n8n users can import directly from the raw file URL:

`https://raw.githubusercontent.com/<GITHUB_OWNER>/<REPO>/main/n8n/workflows/<file>.json`

### Notes on these templates

- **Slack templates**: use an incoming webhook URL (no Slack credential required).
- **Google Sheets template**: requires Google Sheets credentials in n8n (node will show “missing credentials” until configured).
- **Notion template**: requires Notion credentials in n8n and a database with a compatible schema.
- **Email digest template**: requires SMTP credentials in n8n.
  - It uses a Manual Trigger by default (so you can test immediately).

