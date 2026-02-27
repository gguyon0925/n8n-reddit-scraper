# 🔍 Reddit Subreddit Discovery & Niche Audience Map

**Automatically discover relevant subreddits for any niche and generate a complete audience map with AI.**

Enter a few keywords → get back a ranked list of communities, audience profiles, pain points, content opportunities, and engagement patterns — all in Google Sheets.

---

## 🎯 Who is this for?

- **Content marketers** entering a new niche who need to find where their audience lives
- **Community managers** mapping the Reddit landscape for their brand
- **Indie hackers & founders** validating a product idea by finding where potential users hang out
- **Ad buyers** identifying subreddits for Reddit Ads targeting
- **SEO professionals** doing audience research for content strategy

## ❓ What problem does it solve?

Finding relevant subreddits manually is painful — you browse, search, cross-reference, read posts, and try to gauge activity levels. For a new niche, this can take hours. This workflow does it in **2 minutes** and gives you structured, AI-analyzed data you can act on immediately.

## ⚡ What it does (step by step)

1. **You enter niche keywords** (via form or manual trigger)
2. **DISCOVER** — The Reddit Scraper's unique Discover mode finds subreddits matching your keywords (with subscriber counts and activity estimates)
3. **SELECT (AI)** — An AI model picks the top 10 subreddits based on relevance + activity
4. **SCRAPE** — Top posts from the past month are scraped from each selected subreddit, including high-engagement comments
5. **AI ANALYSIS** — GPT-4o-mini analyzes all the data and generates per-subreddit audience profiles
6. **OUTPUT** — A structured Audience Map is saved to Google Sheets

## 📊 What you get (Google Sheets output)

| Column                   | Content                                                  |
| ------------------------ | -------------------------------------------------------- |
| subreddit                | Community name (e.g. r/mealprep)                         |
| communityProfile         | Who's in this community (demographics, experience level) |
| topThemes                | Recurring topics and themes                              |
| painPoints               | Problems and frustrations people express                 |
| contentOpportunities     | Gaps and opportunities for content                       |
| languageTone             | How people communicate (formal/casual, jargon)           |
| engagementPattern        | What types of posts get the most engagement              |
| recommendedContentAngles | 3 specific post ideas for this community                 |

Plus a **Cross-Community Summary** row with overall persona and priority ranking.

## 📋 Prerequisites

| Service            | What you need                            | Cost                                       |
| ------------------ | ---------------------------------------- | ------------------------------------------ |
| **Apify**          | Free account + API token                 | Free tier ($5/mo credits covers ~30+ runs) |
| **Reddit Scraper** | Actor published on Apify Store           | Your actor                                 |
| **OpenAI**         | API key                                  | ~$0.01-0.03 per run (GPT-4o-mini)          |
| **Google Sheets**  | Google account + OAuth2 connected in n8n | Free                                       |
| **n8n**            | Self-hosted (free) or Cloud              | Free (self-hosted) or $20/mo (Cloud)       |

## 🔧 Setup (5 minutes)

### Step 1: Import the workflow

- Download `subreddit-discovery-niche-map.json`
- In n8n: **☰ Menu → Import from File** → select the JSON

### Step 2: Set your Actor ID

- Open the **"Parse Input & Build Config"** node
- Actor ID is already set to `spry_wholemeal~reddit-scraper` by default
- If you fork/clone the Actor, update `actorId` in **"Parse Input & Build Config"** to your own

### Step 3: Paste your Apify API token

- Open the **"Parse Input & Build Config"** node
- Replace `APIFY_TOKEN_HERE` with your Apify API token (Apify Console → Settings → Integrations / API tokens)

### Step 4: Paste your OpenAI API key

- Open the **"Parse Input & Build Config"** node
- Replace `OPENAI_API_KEY_HERE` with your OpenAI API key

### Step 5: Set up Google Sheets

- Create a new Google Sheet with these headers in Row 1:
  ```
  subreddit | communityProfile | topThemes | painPoints | contentOpportunities | languageTone | engagementPattern | recommendedContentAngles
  ```
- Click the **"📊 Save to Google Sheets"** node
- Connect your Google Sheets account
- Paste your sheet URL

### Step 6: Run it!

- Click **"Test workflow"** and enter your niche keywords
- Or activate the workflow and use the Form trigger URL

## 💰 Cost Estimate

| Component                 | Per Run         | Monthly (daily) |
| ------------------------- | --------------- | --------------- |
| Apify (Discover + Scrape) | ~$0.05-0.15     | ~$1.50-4.50     |
| OpenAI (GPT-4o-mini)      | ~$0.01-0.03     | ~$0.30-0.90     |
| **Total**                 | **~$0.06-0.18** | **~$1.80-5.40** |

## 🔄 Customization Ideas

- **Change AI model**: Swap `gpt-4o-mini` for `gpt-4o` in the AI node for deeper analysis (~10x cost)
- **Adjust discovery depth**: Change `maxSubredditsPerKeyword` in the config node (default: 25)
- **Change subscriber threshold**: Adjust `minSubscribers` to find smaller niche communities
- **Add Slack notifications**: Connect a Slack node after the Sheets output to alert your team
- **Schedule it**: Replace the Manual Trigger with a Schedule trigger to track new subreddits weekly
- **Output to Notion**: Replace Google Sheets with Notion for a richer database view

## 🏗️ Technical Notes

- Uses **HTTP Request nodes** (not the Apify community node) for maximum compatibility — works on any n8n instance without installing extra packages
- The `run-sync-get-dataset-items` Apify API endpoint handles run + wait + return in a single call
- All Code nodes include detailed inline comments explaining the logic
- The AI prompt requests **structured JSON** for reliable parsing
- Fallback handling if the AI returns non-JSON responses

## 🔗 Links

- **Reddit Scraper on Apify**: [YOUR_ACTOR_URL]
- **n8n**: [https://n8n.io](https://n8n.io)
- **Apify Console**: [https://console.apify.com](https://console.apify.com)
