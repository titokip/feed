# FastNews permanent article archive

FastNews stores its permanent published-article history in this directory.

## Structure

- `articles/YYYY-MM.json` — one JSON file per UTC publication month.
- Each monthly file contains `schemaVersion`, `month`, and an `articles` array.

Each article record contains:

- `title`
- `category`
- `country`
- `bloggerPostId`
- `url` — the original FastNews Blogger article URL
- `publishedAt` — ISO 8601 timestamp

The publisher appends records through the GitHub Contents API. It checks the Blogger post ID and URL before appending, so retries do not create duplicate entries.

## Retention and operating model

The archive is partitioned by month and is intended to grow without a fixed article-count limit. The Apps Script project keeps only a rolling working cache of the latest 60 article titles in `ARTICLE_HISTORY`, plus its small category/country rotation state. The rolling cache supports prompt context and recent duplicate avoidance; this GitHub archive is the long-term record.

If GitHub is temporarily unavailable, the publisher queues the affected record in the Script Property `FASTNEWS_LEDGER_PENDING` and retries it during a later scheduled run. A ledger write failure does not interrupt an article that has already been published to Blogger.

No API keys, access tokens, or other credentials belong in this archive.
