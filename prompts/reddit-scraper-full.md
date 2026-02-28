# Reddit Scraper — Full Agent Reference

You have access to the Reddit Scraper Apify Actor (`spry_wholemeal/reddit-scraper`) via MCP. This document is your complete reference for using it.

## Overview

Scrapes Reddit's public JSON endpoints (no API key needed). 4 modes: scrape subreddits, search posts, discover subreddits, track domain links. Supports deep comment threading, engagement metrics, strict search, and per-target overrides.

---

## Modes & Required Inputs

### Scrape — Pull posts from specific subreddits

```json
{
  "mode": "scrape",
  "scrape": {
    "subreddits": ["entrepreneur", "startups"],
    "sort": "hot",
    "timeframe": "week",
    "maxPostsPerSubreddit": 100,
    "commentsMode": "none",
    "commentsMaxTopLevel": 50,
    "commentsMaxDepth": 3
  }
}
```

- `subreddits` (string[], required): No `r/` prefix.
- `sort`: `"hot"` (default), `"new"`, `"top"`, `"rising"`, `"controversial"`.
- `timeframe`: Only used with `top`/`controversial`. Values: `"hour"`, `"day"`, `"week"` (default), `"month"`, `"year"`, `"all"`.
- `maxPostsPerSubreddit` (default 100, max 1000): Reddit caps listings at ~1000.

### Search — Find posts matching queries

```json
{
  "mode": "search",
  "search": {
    "queries": ["best CRM software", "project management tools"],
    "sort": "relevance",
    "timeframe": "week",
    "maxPostsPerQuery": 25,
    "restrictToSubreddit": "",
    "commentsMode": "none"
  }
}
```

- `queries` (string[], required): Supports Reddit search syntax (`AND`, `OR`, `author:`, `flair:`, `self:yes`).
- `sort`: `"relevance"` (default), `"hot"`, `"new"`, `"top"`, `"comments"`.
- `timeframe`: Auto-applied only when sort is `"top"`. For other sorts, only sent if explicitly set.
- `restrictToSubreddit`: Limit to one subreddit (no `r/` prefix).
- `maxPostsPerQuery` (default 25, max 1000).

**Filters (optional):**
- `authorFilter`: Only posts by this username.
- `flairFilter`: Only posts with this flair text.
- `selfPostsOnly` (boolean): Only text posts.
- `includeNsfw` (boolean, default false).

**Strict search — exact term matching:**

```json
{
  "mode": "search",
  "search": {
    "queries": ["best CRM software"],
    "strictEnabled": true,
    "strictOperator": "AND",
    "strictTerms": ["voice agent", "sales"]
  }
}
```

This sends: `"best CRM software" AND "voice agent" AND "sales"` to Reddit. Strict terms are appended to every query. Use `"OR"` to match any term instead of all.

### Discover — Find subreddits by keyword

```json
{
  "mode": "discover",
  "discover": {
    "terms": ["saas", "startup tools"],
    "maxSubredditsPerTerm": 25,
    "minSubscribers": 100,
    "estimateActivity": true,
    "includeNsfw": false
  }
}
```

- `terms` (string[], required): Keywords to search.
- `maxSubredditsPerTerm` (default 25, max 100).
- `minSubscribers` (default 100): Filter out tiny subreddits.
- `estimateActivity` (default true): Fetches recent posts to calculate posting frequency (extra requests).

### Domain — Find posts linking to specific websites

```json
{
  "mode": "domain",
  "domain": {
    "domains": ["github.com", "mycompany.com"],
    "sort": "new",
    "timeframe": "week",
    "maxPostsPerDomain": 200,
    "commentsMode": "none"
  }
}
```

- `domains` (string[], required): Full URLs are normalized to domain. Only finds posts that **link out** to the domain (not text mentions).
- `sort`: `"new"` (default), `"hot"`, `"top"`, `"rising"`, `"controversial"`.
- `maxPostsPerDomain` (default 200, max 1000).

---

## Comments

Available in scrape, search, and domain modes. Set at mode level, overridable per target.

| Field | Default | Description |
|-------|---------|-------------|
| `commentsMode` | `"none"` | `"none"` (skip), `"all"` (every post), `"high_engagement"` (thresholds) |
| `commentsMaxTopLevel` | `50` | Top-level comments to fetch. `0` = max (~500). |
| `commentsMaxDepth` | `3` | Reply nesting depth. `0` = top-level only. Max `10`. |

**High engagement mode thresholds:**

| Field | Default | Description |
|-------|---------|-------------|
| `commentsHighEngagementMinScore` | `10` | Post score must be >= this |
| `commentsHighEngagementMinComments` | `5` | Post comment count must be >= this |
| `commentsHighEngagementFilterPosts` | `false` | If `true`, non-qualifying posts are **omitted from the dataset entirely** |

Both thresholds must be met (AND logic).

**Critical limitation:** Reddit returns `"kind":"more"` placeholder nodes for large threads. The scraper does NOT expand these. Use `missing_direct_replies` on comment records to know the lower bound of what's missing.

---

## Per-Target Overrides

Override defaults for specific subreddits, queries, or domains. Any field not specified in an override falls back to the mode-level default.

```json
{
  "mode": "scrape",
  "scrape": {
    "subreddits": ["python", "AskReddit"],
    "sort": "hot",
    "maxPostsPerSubreddit": 100,
    "commentsMode": "none",
    "overrides": [
      {
        "subreddit": "AskReddit",
        "sort": "top",
        "timeframe": "day",
        "maxPostsPerSubreddit": 25,
        "commentsMode": "all"
      }
    ]
  }
}
```

Search overrides use `"query"` as the key. Domain overrides use `"domain"` as the key.

---

## Output — Post Fields

| Field | Type | Description |
|-------|------|-------------|
| `record_type` | `"post"` | Always `"post"` |
| `post_id` | string | Reddit ID (no `t3_` prefix) |
| `subreddit` | string | Subreddit name |
| `title` | string | Post title |
| `text` | string | Post body (empty for link posts) |
| `author` | string | Username |
| `url` | string | External URL or Reddit permalink |
| `permalink` | string | Reddit permalink |
| `domain` | string | e.g. `"github.com"` or `"self.python"` |
| `score` | number | Upvotes minus downvotes |
| `upvote_ratio` | number | 0–1 (e.g. 0.95 = 95% upvoted) |
| `num_comments` | number | Total comments on Reddit (not just fetched) |
| `created_utc_iso` | string | ISO 8601 creation time |
| `scraped_at_iso` | string | ISO 8601 scrape time |

**Engagement metrics (calculated at scrape time):**

| Field | Type | Description |
|-------|------|-------------|
| `age_seconds` | number | Seconds since post creation |
| `score_per_hour` | number | `score / hours_old` — momentum metric |
| `comments_per_hour` | number | `num_comments / hours_old` |
| `estimated_upvotes` | number | Derived from score + upvote_ratio |
| `estimated_downvotes` | number | `estimated_upvotes - score` |
| `total_votes` | number | `estimated_upvotes + estimated_downvotes` |
| `discussion_ratio` | number | `num_comments / total_votes` |
| `engagement_level` | string | `"viral"`, `"high"`, `"medium"`, or `"low"` |
| `is_controversial` | boolean | Upvote ratio near 0.5 |

**Content flags:** `is_self`, `is_video`, `is_original_content`, `over_18`, `spoiler`, `stickied`, `locked`, `archived`, `edited`, `has_question_mark`, `has_url`, `has_media`

**Listing context:** `listing_sort`, `listing_timeframe`, `listing_page`, `listing_rank`

**Attribution (search/domain modes):** `matched_search_queries[]`, `matched_search_targets[]`, `matched_domains[]`, `matched_domain_targets[]`

---

## Output — Comment Fields

| Field | Type | Description |
|-------|------|-------------|
| `record_type` | `"comment"` | Always `"comment"` |
| `comment_id` | string | Reddit ID (no `t1_` prefix) |
| `post_id` | string | Parent post ID |
| `parent_type` | string | `"post"` (top-level), `"comment"` (reply), or `"unknown"` |
| `parent_id` | string | ID of parent (post or comment) |
| `depth` | number | Nesting depth (0 = top-level) |
| `text` | string | Comment body |
| `author` | string | Username |
| `score` | number | Comment score |
| `is_submitter` | boolean | Author is the post author |
| `permalink` | string | URL to comment |
| `created_utc_iso` | string | ISO 8601 creation time |

**Thread completeness:**

| Field | Type | Description |
|-------|------|-------------|
| `reply_count_direct` | number | Direct child comments **we fetched** |
| `reply_count_total` | number | All descendants **we fetched** |
| `missing_direct_replies` | number | Lower bound of missing direct replies. `0` does NOT guarantee completeness. `> 0` = definitely incomplete. |

**To reconstruct threads:** Filter by `post_id`. Top-level comments have `parent_type === "post"`. Build tree using `parent_id`. Use `depth` for indentation.

---

## Output — Subreddit Fields (Discover mode only)

| Field | Type | Description |
|-------|------|-------------|
| `record_type` | `"subreddit"` | Always `"subreddit"` |
| `name` | string | Subreddit identifier |
| `display_name` | string | Display name |
| `title` | string | Sidebar header |
| `public_description` | string | Short description |
| `subscribers` | number | Subscriber count |
| `active_users` | number | Currently active users |
| `estimated_posts_per_day` | number | Calculated if `estimateActivity: true` |
| `estimated_posts_per_week` | number | Calculated if `estimateActivity: true` |
| `subreddit_type` | string | `"public"`, `"private"`, or `"restricted"` |
| `over_18` | boolean | NSFW subreddit |
| `search_term` | string | Term that discovered this subreddit |
| `search_rank` | number | Rank in results for that term |

---

## Important Behaviors & Gotchas

1. **Deduplication:** Posts are deduped by `post_id` across targets. In scrape mode, comments are NOT deduped — if the same post appears in two subreddits and both fetch comments, you get duplicate comment records.

2. **Cross-target comment depth:** In search/domain modes, if a post matches multiple targets, comments use the MAX `maxTopLevel` and `maxDepth` across all matching targets.

3. **`num_comments` vs fetched comments:** `num_comments` on posts is Reddit's total count. The actual comments in your dataset depend on `commentsMaxTopLevel` and `commentsMaxDepth` settings.

4. **`missing_direct_replies == 0` is not a completeness guarantee.** Reddit can omit replies without leaving a placeholder.

5. **Domain mode finds link posts only.** `domain: ["github.com"]` returns posts where the URL points to github.com, NOT text posts mentioning it.

6. **Timeframe is ignored for most search sorts.** Only auto-applied when sort is `"top"`. For other sorts, it's only sent if explicitly set on the target.

7. **`filterPosts: true` in high engagement mode removes non-qualifying posts entirely** from the dataset, not just their comments.

8. **Private/quarantined subreddits return empty results** without error.

9. **Reddit caps listings at ~1000 items.** Setting `maxPostsPerSubreddit` above 1000 won't return more.

10. **All targets run in parallel.** Adding more subreddits/queries doesn't significantly increase total runtime. Comments are the slow/expensive part.

---

## Proxy & Rate Limiting

- Residential proxies are used by default (recommended — Reddit blocks datacenter IPs).
- Request delay: 100ms default + random jitter. Increase to 200–500ms if hitting 429s.
- Auto-retry: 3 attempts with exponential backoff (1s, 2s, 4s).
- 429 responses: Waits `Retry-After` duration (default 60s), rotates proxy IP, retries.
- `proxyCountry` (default `"US"`): Change to `"GB"`, `"DE"`, `"CA"` if US IPs are congested.

---

## Cost & Performance Tips

- **Comments are the expensive part.** Start with `commentsMode: "none"` and add comments only when needed.
- **Use `high_engagement` mode** to fetch comments only on popular posts (saves API calls).
- **Keep `maxPosts*` low while testing** (25–50). Scale up once you've confirmed the output is what you need.
- **`estimateActivity` in discover mode** makes extra requests per subreddit. Disable if you just need names and subscriber counts.
