# Reddit Scraper — Quick Reference

You have the Reddit Scraper Actor (`spry_wholemeal/reddit-scraper`) available via MCP. It scrapes Reddit without API keys. Here's how to use it.

## 4 Modes

### Scrape subreddits

```json
{
  "mode": "scrape",
  "scrape": {
    "subreddits": ["startups", "SaaS"],
    "sort": "hot",
    "maxPostsPerSubreddit": 25
  }
}
```

Sort options: `"hot"` (default), `"new"`, `"top"`, `"rising"`, `"controversial"`. For `top`/`controversial`, add `"timeframe": "week"` (or `hour`, `day`, `month`, `year`, `all`).

### Search posts

```json
{
  "mode": "search",
  "search": {
    "queries": ["best CRM software"],
    "maxPostsPerQuery": 25,
    "sort": "relevance"
  }
}
```

Sort options: `"relevance"` (default), `"hot"`, `"new"`, `"top"`, `"comments"`. Add `"restrictToSubreddit": "startups"` to search within one subreddit.

**Strict search** (exact terms): Add `"strictEnabled": true, "strictOperator": "AND", "strictTerms": ["exact phrase"]` to require specific phrases in results.

### Discover subreddits

```json
{
  "mode": "discover",
  "discover": {
    "terms": ["saas", "developer tools"],
    "maxSubredditsPerTerm": 25,
    "minSubscribers": 100
  }
}
```

Returns subreddit name, subscribers, active users, description, and estimated posting frequency.

### Track domain links

```json
{
  "mode": "domain",
  "domain": {
    "domains": ["github.com"],
    "maxPostsPerDomain": 100,
    "sort": "new"
  }
}
```

Finds posts that **link to** the domain (not text mentions).

## Adding Comments

Add these fields to any scrape/search/domain input:

```json
{
  "commentsMode": "all",
  "commentsMaxTopLevel": 50,
  "commentsMaxDepth": 3
}
```

- `"none"` (default, fastest) — skip comments
- `"all"` — fetch for every post
- `"high_engagement"` — only posts with score >= 10 AND comments >= 5

Comments are the slow/expensive part. Start with `"none"` and add when needed.

## Key Output Fields

**Posts:** `title`, `text`, `author`, `score`, `upvote_ratio`, `num_comments`, `permalink`, `created_utc_iso`, `score_per_hour`, `comments_per_hour`, `engagement_level` (`viral`/`high`/`medium`/`low`)

**Comments:** `text`, `author`, `score`, `depth`, `parent_id`, `parent_type` (`post` or `comment`), `is_submitter`, `reply_count_direct`, `missing_direct_replies`

**Subreddits (discover):** `name`, `subscribers`, `active_users`, `estimated_posts_per_day`, `public_description`

## Things to Know

- No `r/` prefix on subreddit names.
- `num_comments` on posts is Reddit's total — your dataset may have fewer depending on depth/limit settings.
- Reddit caps at ~1000 posts per listing. Setting higher limits won't help.
- All targets run in parallel — more subreddits/queries doesn't mean much slower.
- Reddit returns incomplete comment threads for large posts. Check `missing_direct_replies` to know if replies are missing.
- Keep limits low while testing (25–50 posts). Scale up once output looks right.
