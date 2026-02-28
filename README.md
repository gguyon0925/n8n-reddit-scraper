# Reddit Scraper — Integrations & Automations

Ready-to-use integrations for the [Reddit Scraper](https://apify.com/spry_wholemeal/reddit-scraper) Apify Actor.

## MCP Server Setup

Connect the Reddit Scraper directly to your AI coding assistant (Claude, Cursor, VS Code, Windsurf). One config, full access to all 4 scraper modes.

**[Setup guide →](./mcp/)**

Quickest option — Claude Code, one command:

```bash
claude mcp add reddit-scraper \
  -e APIFY_TOKEN=<YOUR_APIFY_TOKEN> \
  -- npx -y @apify/actors-mcp-server@latest --actors spry_wholemeal/reddit-scraper
```

## AI Agent Prompts

Copy-paste prompts that teach your AI assistant how to use the Reddit Scraper Actor. Add them as project context (`.claude/`, `.cursor/rules/`, etc.) so your agent knows every input, output, and gotcha.

**[Browse prompts →](./prompts/)**

| Prompt | Lines | Best for |
|--------|-------|----------|
| [Full reference](./prompts/reddit-scraper-full.md) | ~350 | Power users, complex workflows |
| [Quick reference](./prompts/reddit-scraper-quick.md) | ~100 | Simple scraping, getting started |

## n8n Workflow Templates

Import-ready n8n workflows — leads, audience research, content generation, and weekly digests. No code required.

**[Browse templates →](./n8n/)**

| Template | What it does |
|----------|-------------|
| Lead Finder | AI buying-intent scanner → Slack + Google Sheets |
| Subreddit Discovery | Find niche subreddits → AI audience map |
| Content Machine | Reddit threads → blog post drafts |
| Weekly Digest | Email summary of top posts + AI trends |

## Sample Outputs

See what these workflows produce: [`examples/`](./examples/)
