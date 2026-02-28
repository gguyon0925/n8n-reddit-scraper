# n8n Workflow Templates — Reddit Scraper

Import-ready n8n workflows that use the [Reddit Scraper](https://apify.com/spry_wholemeal/reddit-scraper) Actor as the data source. No coding required — import, set credentials, run.

## Quick Start

1. Run n8n locally: `npx n8n` (opens `http://localhost:5678`)
2. Import a workflow JSON below (file or URL)
3. Set your credentials in the config node (Apify token, OpenAI key, etc.)
4. Hit **Execute** or **Activate** for scheduling

## Templates

### Lead Finder — AI Buying-Intent Scanner

Finds high-intent Reddit posts and routes them to Slack + Google Sheets.

- **Setup guide:** [`reddit-lead-finder-buying-intent.md`](./reddit-lead-finder-buying-intent.md)
- **Scraper mode:** `search`
- **Cost:** ~$0.12–0.38/run
- **Import URL:**
  ```
  https://raw.githubusercontent.com/gguyon0925/n8n-reddit-scraper/main/n8n/reddit-lead-finder-buying-intent.json
  ```

### Subreddit Discovery — Niche Audience Map

Discovers relevant subreddits, scrapes top posts, generates an AI audience map.

- **Setup guide:** [`subreddit-discovery.md`](./subreddit-discovery.md)
- **Scraper modes:** `discover` + `scrape`
- **Cost:** ~$0.06–0.18/run
- **Import URL:**
  ```
  https://raw.githubusercontent.com/gguyon0925/n8n-reddit-scraper/main/n8n/subreddit-discovery.json
  ```

### Content Machine — Blog Drafts from Discussions

Turns discussion-rich Reddit threads into outlines and full blog post drafts.

- **Setup guide:** [`README-content-machine.md`](./README-content-machine.md)
- **Scraper mode:** `search`
- **Cost:** ~$0.04–0.11/post
- **Import URL:**
  ```
  https://raw.githubusercontent.com/gguyon0925/n8n-reddit-scraper/main/n8n/reddit-content-machine-blog-generator.json
  ```

### Weekly Digest Email

Weekly email summary of top posts with optional AI trend analysis.

- **Scraper mode:** `search`
- **Import URL:**
  ```
  https://raw.githubusercontent.com/gguyon0925/n8n-reddit-scraper/main/n8n/reddit-weekly-digest-email.json
  ```

## Prerequisites

- [Apify account](https://apify.com/) (free tier works)
- [OpenAI API key](https://platform.openai.com/) (for AI-powered workflows)
- Google Sheets / Slack (depending on template)
- n8n ([cloud](https://n8n.io/) or local via `npx n8n`)
