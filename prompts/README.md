# AI Agent Prompts — Reddit Scraper

Copy-paste prompts that teach your AI coding assistant how to use the [Reddit Scraper](https://apify.com/spry_wholemeal/reddit-scraper) Actor effectively. These are designed to be added as system context, project rules, or pasted directly into a conversation.

## Prerequisites

Set up the MCP server first: **[MCP Setup Guide](../mcp/)**

## Prompts

### Full Reference Prompt

The complete Actor reference — input schemas, output schemas, all modes, comment threading, engagement metrics, edge cases, and gotchas. Use this when you want your agent to have deep knowledge of every capability.

**Best for:** Power users, complex workflows, building automations on top of the Actor.

- [`reddit-scraper-full.md`](./reddit-scraper-full.md) — ~350 lines, covers everything

### Quick Reference Prompt

A condensed version with the most common inputs, outputs, and tips. Covers all 4 modes but skips advanced features like per-target overrides, proxy tuning, and thread reconstruction.

**Best for:** Quick scraping tasks, simple searches, getting started.

- [`reddit-scraper-quick.md`](./reddit-scraper-quick.md) — ~100 lines, 80/20 coverage

## How to Use

### Claude Code

Add as a project instruction file:

```bash
# Copy into your project (Claude Code auto-reads CLAUDE.md and .claude/ files)
cp reddit-scraper-full.md /path/to/your/project/.claude/reddit-scraper.md
```

Or paste directly into a conversation when you need it.

### Cursor

Add as a project rule. In your project root, create `.cursor/rules/reddit-scraper.mdc`:

```
---
description: Reddit Scraper Actor reference for MCP tool usage
globs:
alwaysApply: true
---

<paste prompt content here>
```

### VS Code (GitHub Copilot)

Add as a Copilot instruction file. Create `.github/copilot-instructions.md` in your project root and paste the prompt content.

### General

These prompts work with any AI assistant. Paste them at the start of a conversation or add them to your tool's system prompt configuration.
