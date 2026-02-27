# Reddit to AI Content Machine — Blog Posts from Real Discussions

**Turn Reddit conversations into blog post drafts automatically. Enter a niche topic, get back 5+ fully-written blog posts grounded in real questions, pain points, and experiences people are sharing.**

The output: a complete content pipeline in Google Sheets with SEO-optimized titles, keywords, full Markdown drafts, and source attribution — ready for editing and publishing.

---

## Who is this for?

- **Content marketers** who need a steady stream of topic ideas grounded in real audience questions
- **Bloggers and creators** who want to write about what people actually care about, not what they guess
- **SEO professionals** looking for content angles backed by real search intent
- **Agency teams** producing content at scale for clients across multiple niches
- **Indie hackers and founders** building content marketing engines for their products

## What problem does it solve?

The hardest part of content marketing isn't writing — it's knowing what to write about. Most content teams either guess at topics, chase keywords blindly, or spend hours manually reading forums to understand what their audience cares about. This workflow automates the entire research-to-draft pipeline: it finds the richest discussions in your niche, identifies the best content angles from real conversations, and generates publication-ready drafts.

## How it works

```
Content Brief Form (niche, audience, tone, # posts)
    |
    v
Build Search Config — generates 8 query variations per topic:
  "remote work tips", "remote work how do you",
  "remote work unpopular opinion", "remote work mistake", etc.
    |
    v
Search Reddit (Apify) — fetches top posts with FULL comments
  (30 comments, 3 levels deep — the gold is in the replies)
    |
    v
Filter Discussion-Rich Threads — requires 10+ comments AND 5+ upvotes
  Scores by: (comments x 3) + (avg comment length x 0.1) + (score x 0.5)
  Selects top threads ranked by content richness, not just popularity
    |
    v
AI Generate Outlines (Step 1) — analyzes threads, creates structured
  outlines: SEO title, hook, sections with key points, target keywords,
  unique angle, content type classification
    |
    v
AI Write Draft (Step 2) — writes full ~1200-word Markdown blog post
  from each outline, with anti-cliche prompting for authentic voice
    |
    v
Google Sheets — complete content pipeline with drafts + metadata
Slack (optional) — summary notification
```

## Why two-step AI?

This workflow uses a 2-step approach that mirrors how professional writers work:

**Step 1 — Outline (temp 0.4, analytical):** The AI acts as a content strategist. It reads all the threads, identifies the best angles, avoids topic overlap, assigns SEO keywords, and structures each post with section headings and key points.

**Step 2 — Draft (temp 0.6, creative):** The AI acts as a writer. Working from the structured outline, it produces engaging prose with the specified tone. The outline scaffold prevents the common "AI rambling" problem.

This consistently produces better content than a single "write me a blog post about X" prompt.

## What you get (Google Sheets output)

| Column | Example |
|--------|---------|
| title | "7 Remote Work Habits That Actually Stick (From People Who've Done It)" |
| slug | 7-remote-work-habits-that-actually-stick |
| contentType | listicle |
| targetKeyword | remote work habits |
| secondaryKeywords | work from home tips, remote productivity, home office routine |
| uniqueAngle | Sourced from real long-term remote workers, not theoretical advice |
| estimatedWordCount | 1,247 |
| blogDraft | Full Markdown text — copy into your CMS |
| sourceThreads | Reddit URLs that inspired the post (internal ref only) |
| status | Draft - Needs Review |

## Prerequisites

| Service | What you need | Cost |
|---------|--------------|------|
| **Apify** | Free account + API token | Free tier ($5/mo) |
| **Reddit Scraper** | Your actor on Apify Store | Your actor |
| **OpenAI** | API key | ~$0.05-0.15/run for 5 posts |
| **Google Sheets** | Google account + OAuth2 | Free |
| **n8n** | Self-hosted or Cloud | Free (self-hosted) or $20/mo |
| **Slack** | Optional — for summary notifications | Free |

## Setup (5 minutes)

### Step 1: Import the workflow
- Download `reddit-content-machine-blog-generator.json`
- In n8n: **Menu → Import from File** → select the JSON

### Step 2: Set your Actor ID
- Open **"Build Search Config"** code node
- Replace `YOUR_ACTOR_ID` with your Apify Actor ID

### Step 3: Configure Apify credentials
- Click **"Search Reddit (Apify)"** node
- Create Header Auth credential:
  - Header Name: `Authorization`
  - Header Value: `Bearer YOUR_APIFY_API_TOKEN`

### Step 4: Configure OpenAI
- Click **"AI Generate Outlines"** node → add OpenAI API key
- Click **"AI Write Draft"** node → use same credential

### Step 5: Set up Google Sheets
Create a sheet with a tab named **"Content Pipeline"** and these headers:
```
title | slug | contentType | targetKeyword | secondaryKeywords | uniqueAngle | estimatedWordCount | blogDraft | sourceThreads | sourceSubreddits | niche | audience | tone | status | generatedAt
```
Paste the sheet URL into the Google Sheets node.

### Step 6: Run it!
- Click **"Test workflow"** to use manual trigger, or
- Use the Form trigger URL for a nice input interface

## Cost per run

| Component | 5 Posts | 10 Posts |
|-----------|---------|----------|
| Apify (search + deep comments) | ~$0.15-0.40 | ~$0.25-0.60 |
| OpenAI outlines (1 call) | ~$0.02-0.05 | ~$0.03-0.08 |
| OpenAI drafts (per post) | ~$0.01-0.03 each | ~$0.01-0.03 each |
| **Total** | **~$0.20-0.55** | **~$0.35-0.95** |

That's roughly **$0.04-0.11 per blog post draft** — cheaper than a cup of coffee.

## Customization ideas

**Change the search strategy:**
Edit the query patterns in "Build Search Config" to match your content strategy. Current patterns find tips, experiences, opinions, mistakes, and transformation stories. Add patterns like `"[topic] vs"` for comparison posts or `"[topic] 2026"` for timely content.

**Upgrade AI quality:**
Swap `gpt-4o-mini` for `gpt-4o` in the Draft node for noticeably better writing (~10x cost, still cheap at ~$0.10-0.30 per post). Keep the Outline node on `gpt-4o-mini` since structured analysis is its strength.

**Add scheduling:**
Replace the Manual Trigger with a Schedule Trigger (weekly) to auto-generate content on a cadence.

**Pipe to WordPress:**
Add an HTTP Request node after the Sheets node to POST drafts to your WordPress REST API as draft posts.

**Add human review:**
Insert an n8n Wait node before publishing that pauses until you approve each draft via a webhook.

**Best niches for this workflow:**
Topics where people share experiences, ask questions, and debate solutions. B2B SaaS, personal finance, career advice, health/fitness, home improvement, dev tools, productivity. Less effective for meme-heavy or purely visual communities.

## Technical notes

- **Deep comment scraping** is key — we fetch 30 comments at 3 levels of depth because the real insights are in the discussion threads, not just the original post
- **Content richness scoring** prioritizes threads with many substantive comments over viral posts with low-effort replies
- **Parse Outlines** splits the AI response into individual items so n8n auto-loops the Draft node — each post is written as a separate AI call for better quality
- **Fan-out connection** from Format Content Output sends each post to both Sheets and the Summary builder simultaneously
- **Anti-cliche prompting** in the Draft step specifically instructs the AI to avoid common AI writing patterns ("In today's world...", "Have you ever wondered...")

## Links

- **Reddit Scraper on Apify**: `https://console.apify.com/actors/spry_wholemeal~reddit-scraper` (replace with your Actor if different)
- **n8n**: [https://n8n.io](https://n8n.io)
- **Apify Console**: [https://console.apify.com](https://console.apify.com)
