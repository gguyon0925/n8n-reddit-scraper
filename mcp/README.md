# MCP Server Setup — Reddit Scraper

Connect the [Reddit Scraper](https://apify.com/spry_wholemeal/reddit-scraper) Actor to your AI coding assistant. Your agent gets direct access to Reddit scraping, search, subreddit discovery, and domain tracking.

## Prerequisites

- **Node.js 18+** (for `npx`)
- **Apify API token** — get yours at [Settings > Integrations](https://console.apify.com/account/integrations)

---

## Claude Code

Run this in your terminal:

```bash
claude mcp add reddit-scraper \
  -e APIFY_TOKEN=<YOUR_APIFY_TOKEN> \
  -- npx -y @apify/actors-mcp-server@latest --actors spry_wholemeal/reddit-scraper
```

That's it. Claude Code will confirm the server was added.

---

## Claude Desktop

Edit your config file:

| OS | Path |
|----|------|
| macOS | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| Windows | `%APPDATA%\Claude\claude_desktop_config.json` |

Paste this (or download [`claude-desktop.json`](./claude-desktop.json) and merge it into your existing config):

```json
{
  "mcpServers": {
    "reddit-scraper": {
      "command": "npx",
      "args": [
        "-y",
        "@apify/actors-mcp-server@latest",
        "--actors",
        "spry_wholemeal/reddit-scraper"
      ],
      "env": {
        "APIFY_TOKEN": "<YOUR_APIFY_TOKEN>"
      }
    }
  }
}
```

Fully quit and restart Claude Desktop. A plug icon in the bottom-right confirms it's active.

---

## Cursor

Add to `.cursor/mcp.json` in your project root (or open **Settings > MCP**):

```json
{
  "mcpServers": {
    "reddit-scraper": {
      "command": "npx",
      "args": [
        "-y",
        "@apify/actors-mcp-server@latest",
        "--actors",
        "spry_wholemeal/reddit-scraper"
      ],
      "env": {
        "APIFY_TOKEN": "<YOUR_APIFY_TOKEN>"
      }
    }
  }
}
```

Or download [`cursor.json`](./cursor.json) and save it as `.cursor/mcp.json`.

---

## VS Code (GitHub Copilot)

Requires GitHub Copilot. Open Command Palette (`Cmd+Shift+P` / `Ctrl+Shift+P`) > **MCP: Open User Configuration**, then add:

```json
{
  "mcpServers": {
    "reddit-scraper": {
      "command": "npx",
      "args": [
        "-y",
        "@apify/actors-mcp-server@latest",
        "--actors",
        "spry_wholemeal/reddit-scraper"
      ],
      "env": {
        "APIFY_TOKEN": "<YOUR_APIFY_TOKEN>"
      }
    }
  }
}
```

Or download [`vscode.json`](./vscode.json) and paste its contents.

---

## Windsurf

Add to `~/.codeium/windsurf/mcp_config.json`:

```json
{
  "mcpServers": {
    "reddit-scraper": {
      "command": "npx",
      "args": [
        "-y",
        "@apify/actors-mcp-server@latest",
        "--actors",
        "spry_wholemeal/reddit-scraper"
      ],
      "env": {
        "APIFY_TOKEN": "<YOUR_APIFY_TOKEN>"
      }
    }
  }
}
```

Or download [`windsurf.json`](./windsurf.json) and merge it in.

---

## What your agent can do once connected

| Capability | Example prompt |
|------------|---------------|
| Scrape subreddits | "Scrape the top 50 posts from r/startups this week" |
| Search Reddit | "Search Reddit for posts about 'best CRM software'" |
| Discover subreddits | "Find subreddits related to SaaS and developer tools" |
| Track domains | "Find all Reddit posts linking to mycompany.com" |
| Fetch comments | "Get that post and all its comments" |

All 4 scraper modes (`scrape`, `search`, `discover`, `domain`) are available, with full comment threading, engagement metrics, and filtering support.
