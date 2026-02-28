# 🎯 Reddit Lead Finder — AI Pain Point & Buying Intent Scanner

**Automatically scans Reddit for people actively looking for a product/service like yours, scores them by buying intent with AI, and alerts your team daily with the hottest leads.**

Set it up once → get qualified leads in Slack every morning.

---

## 🎯 Who is this for?

- **SaaS founders** looking for early adopters and validation
- **Indie hackers** finding their first customers from real pain points
- **Sales teams** at B2B companies who need warm Reddit leads
- **Agencies** doing social selling for clients
- **Freelancers** finding people who need their exact service

## ❓ What problem does it solve?

There are thousands of Reddit posts every day from people saying "I need a tool for X", "is there an alternative to Y?", or "I'm frustrated with Z". Finding these posts manually is a full-time job. This workflow finds them automatically, scores how likely each person is to buy, and delivers only the good ones to your Slack.

## ⚡ How it works

```
⏰ Daily Schedule
    ↓
⚙️ Config (your product info + search queries)
    ↓
🔍 Search Reddit — hunts for buying-intent phrases:
   "looking for tool", "alternative to [competitor]",
   "frustrated with", "anyone recommend", etc.
    ↓
📈 Deduplicate & Enrich — removes duplicates, calculates
   engagement velocity (score/hour), filters low quality
    ↓
🤖 AI Lead Scoring — GPT-4o-mini scores each post 0-100
   against YOUR specific product. Identifies: pain point,
   intent signals, relevance, suggested response angle
    ↓
🔥 Route by score:
   ├── Score ≥ 65 (HOT) → Google Sheets + Slack Alert
   └── Score 40-64 (WARM) → Google Sheets only
```

## 📊 What each lead includes

| Field | Example |
|-------|---------|
| leadScore | 82 |
| buyingIntent | "high" |
| painPoint | "Needs automated invoice tracking, currently doing it manually in spreadsheets" |
| intentSignals | "actively seeking tool \| mentioned budget \| comparing options" |
| relevanceToProduct | "Your invoicing feature directly solves their spreadsheet pain" |
| suggestedResponse | "Share how automated reconciliation works without pushing product directly" |
| suggestedAction | "prioritize" |
| subreddit | r/smallbusiness |
| scorePerHour | 12.5 (hot engagement velocity) |
| engagementLevel | "rising" |
| url | Direct link to the post |

## 📋 Prerequisites

| Service | What you need | Cost |
|---------|--------------|------|
| **Apify** | Free account + API token | Free tier ($5/mo credits) |
| **Reddit Scraper** | Your actor on Apify Store | Your actor |
| **OpenAI** | API key | ~$0.02-0.08/run (GPT-4o-mini) |
| **Google Sheets** | Google account + OAuth2 | Free |
| **Slack** | Workspace + OAuth2 app | Free (or use Email/Discord/Telegram instead) |
| **n8n** | Self-hosted or Cloud | Free (self-hosted) or $20/mo |

## 🔧 Setup (10 minutes)

### Step 1: Import the workflow
- Download `reddit-lead-finder-buying-intent.json`
- In n8n: **☰ Menu → Import from File** → select the JSON

### Step 2: Edit the Config node (most important!)
Open **"⚙️ Lead Finder Config"** and customize:

```javascript
const productContext = {
  name: 'InvoiceBot',                           // Your product name
  description: 'Automated invoice processing',   // What you do
  targetAudience: 'Small business owners',        // Who buys it
  keyFeatures: ['auto-reconciliation', 'OCR'],   // Key features
  competitors: ['FreshBooks', 'QuickBooks']       // Competitors
};
```

The competitors auto-generate searches like "alternative to FreshBooks", "switching from QuickBooks", etc.

Actor ID is already set to `spry_wholemeal~reddit-scraper` by default. If you fork/clone the Actor, update the `actorId` in the Config node.

### Step 3: Paste your Apify API token
- Open **"⚙️ Lead Finder Config"**
- Replace `APIFY_TOKEN_HERE` with your Apify API token (Apify Console → Settings → Integrations / API tokens)

### Step 4: Configure OpenAI
- Click **"🤖 AI Lead Scoring"** node
- Add your OpenAI API key

### Step 5: Create Google Sheet
Create a sheet with **two tabs**:
- Tab 1: **"Hot Leads"**
- Tab 2: **"Warm Leads"**

Both tabs need these column headers in Row 1:
```
leadScore | buyingIntent | suggestedAction | intentSignals | painPoint | relevanceToProduct | suggestedResponse | subreddit | title | url | author | postScore | numComments | scorePerHour | engagementLevel | hoursOld | matchedQuery | selftext | scannedAt
```

Paste the sheet URL into both Google Sheets nodes.

### Step 6: Connect Slack (or alternative)
- Click **"📢 Slack Alert"** node
- Connect your Slack workspace
- Change `#leads` to your channel name

**Don't use Slack?** Replace with Gmail, Discord, Telegram, or Teams — the digest message works with any channel.

### Step 7: Activate!
- Click **"Active"** toggle in the top-right
- The workflow runs daily automatically
- Use **"Test workflow"** button for immediate testing

## 💰 Cost Estimate

| Component | Per Run | Monthly (daily) |
|-----------|---------|-----------------|
| Apify (Search mode) | ~$0.10-0.30 | ~$3-9 |
| OpenAI (GPT-4o-mini) | ~$0.02-0.08 | ~$0.60-2.40 |
| **Total** | **~$0.12-0.38** | **~$3.60-11.40** |

Fits comfortably within Apify's free $5/month credit for lighter configurations.

## 🔄 Customization Ideas

**Adjust sensitivity:**
- Lower the filter threshold to 50 for more leads (noisier)
- Raise to 75 for fewer, higher-confidence leads only

**Add niche-specific search queries:**
In the Config node, add industry-specific phrases your customers use:
```javascript
'spreadsheet for tracking clients',   // Your niche pain point
'CRM too expensive for small team',   // Budget signal
'how do you handle invoicing'         // Process question
```

**Add subreddit filtering:**
In the Enrich node, add an allowlist or blocklist of subreddits to focus or exclude.

**Track conversions:**
Add a manual "status" column to your Sheet (contacted / replied / converted) and review monthly to optimize your queries.

**Increase frequency:**
Change the Schedule trigger from 24h to 6h or 12h for faster lead response.

**Upgrade AI model:**
Swap `gpt-4o-mini` for `gpt-4o` for ~3-5% better scoring accuracy at ~10x cost.

## 🏗️ Technical Notes

- **HTTP Request nodes** used instead of Apify community node — no install needed
- **Batched AI processing** — posts are scored in groups of 10 to stay within token limits
- **Engagement velocity** (`scorePerHour`) is the secret weapon — a post gaining 50 upvotes in 2 hours is far hotter than 50 over 2 weeks. Only your scraper provides this metric.
- **Dual-output filter** — the Filter node routes hot leads (TRUE output) to both Sheets and Slack, warm leads (FALSE output) to Sheets only
- **Graceful error handling** — if AI returns non-JSON, the workflow continues without crashing
- **Deduplication built in** — same post matching multiple queries is only scored once

## 🔗 Links

- **Reddit Scraper on Apify**: `https://console.apify.com/actors/spry_wholemeal~reddit-scraper` (replace with your Actor if different)
- **n8n**: [https://n8n.io](https://n8n.io)
- **Apify Console**: [https://console.apify.com](https://console.apify.com)
