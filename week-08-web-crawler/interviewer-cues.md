# Week 08: Interviewer cues — Web Crawler

## What they're listening for

| Signal | What it means |
|---|---|
| You name the URL frontier as the centerpiece | "Has read the Mercator paper or a derivative." |
| You enforce politeness per-domain, not globally | "Knows the real-world constraint." |
| You bring up robots.txt + crawl-delay | "Knows the web's etiquette layer." |
| You use a Bloom filter for URL dedup | "Notices the memory tradeoff." |
| You use SimHash (or MinHash) for content dedup | "Knows near-duplicates are 30% of the web." |
| You mention DNS caching | "Has actually run a crawler." |
| You flag spider traps unprompted | "Has been bitten." |
| You make the frontier persistent | "Thinks about 30-day jobs." |

## Yellow flags

- One global queue (no per-domain isolation).
- Exact-match dedup only (misses near-duplicates).
- "We'll cache DNS" without explaining why it matters.
- No way to resume from crash.
- Storing every URL in a hash set with no Bloom filter consideration.

## Red flags

- Suggesting a SQL `SELECT next URL` per worker (locking nightmare).
- "I'd use Selenium for every page" (kills throughput; only do this for JS-heavy sites).
- Ignoring robots.txt entirely.
- Synchronous crawl-then-index pipeline (Kafka decouples for a reason).
- "Just put it all in MongoDB" with no explanation.

## The killer follow-up

**"What if a site has 10 million pages but you don't want to crawl all of them?"**

This is the politeness + budget question.

- Per-domain crawl cap (default ~100k pages).
- Quality scoring: weight URLs by PageRank, freshness, or content signals; prefer high-score pages.
- Per-domain budget refreshed periodically (don't permanently lock out a site).
- If continuous: re-crawl frequency proportional to observed change rate.

**"What if you discover that a URL you've already fetched has changed content?"**

The re-crawl scheduling problem.

- Track per-URL `last_modified` hash.
- Recrawl frequency = function of change frequency (sites that change often → crawl often).
- HTTP `If-Modified-Since` to short-circuit unchanged pages.

**"How do you handle a site you're not allowed to crawl?"**

- robots.txt is authoritative. Fetched once per domain, cached, refreshed daily.
- If `Disallow: /`, the domain is skipped entirely.
- `Crawl-Delay` overrides our default 1 RPS.
- `Sitemap` URLs are honored as seeds.

## What "senior" looks like vs. "mid-level"

- **Mid-level** designs a fetch fleet with a queue.
- **Senior** centerpieces the frontier, names per-domain politeness as the hard constraint, picks Bloom + SimHash for the two dedup layers, and flags spider traps and DNS as the real-world failure modes.
