## Publishing workflow templates

This repo is for **exported n8n workflow JSON** files that use your Apify Actor.

### Quick import (recommended)

In n8n:

1. Open the workflow canvas
2. Click the menu (three dots) → **Import from File** (or **Import from URL**)
3. Import one of the JSON files from `workflows/`
4. Open the workflow’s **Config (edit me)** node and replace placeholder values:
   - `APIFY_TOKEN_HERE`
   - your Actor ID (defaults to `spry_wholemeal~reddit-scraper` in these templates)
   - destination settings (Slack / Google Sheet / Email / Notion)

These templates intentionally avoid embedding credentials inside nodes, so they are safe to publish publicly.

### Create a template

1. Run n8n locally (free): `npx n8n`
2. Build your workflow (typically: Trigger → HTTP Request (Apify) → Filter/Format → Destination)
3. Export the workflow JSON and save it under `workflows/`

### Share as “Import from URL”

After pushing to GitHub, users can import directly from the raw file URL:

`https://raw.githubusercontent.com/<GITHUB_OWNER>/<REPO>/main/workflows/<file>.json`

### Notes

- **Slack templates**: use an incoming webhook URL (no Slack credential required).
- **Google Sheets templates**: require Google Sheets credentials in n8n.
- **Email templates**: require SMTP credentials in n8n.

