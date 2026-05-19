# Week 07: Interviewer cues — Search Autocomplete

## What they're listening for

| Signal | What it means |
|---|---|
| You pick a trie immediately and explain why | "Knows the canonical data structure for prefix queries." |
| You store top-K *at* each node, not compute on the fly | "Has thought through the read-path cost." |
| You separate the build pipeline from the serving layer | "Knows that batch indexing is a real thing." |
| You bring up atomic snapshot reload | "Has shipped to production." |
| You mention CDN caching at the edge | "Thinks about read-path offload reflexively." |
| You acknowledge real-time deltas as an extension | "Knows when to ship the simpler thing first." |
| You ask about personalization and language | "Recognizes the unstated constraints." |

## Yellow flags

- Designing the trie but not pre-storing top-K (subtree DFS at query time).
- Real-time index updates as the default, when daily was the brief.
- Inverted index for autocomplete (works, but heavyweight; trie is the answer).
- One enormous serving node (no replication or geo-distribution).
- Not noticing that the same prefix repeated many times is the perfect CDN use case.

## Red flags

- "I'd use Elasticsearch" without explaining why a trie is wrong.
- Storing all queries in a SQL table and doing `LIKE 'prefix%'` lookups.
- Designing a write-through architecture where every query updates the index synchronously.
- Treating "daily refresh" as if it required complex incremental indexing.

## The killer follow-up

**"What if a brand-new query goes viral and needs to appear in autocomplete within minutes?"**

This is the daily-batch vs. real-time tradeoff.

- Daily batch alone: the query won't show up until tomorrow.
- Solution: add a real-time *delta* trie fed from a streaming pipeline (Kafka).
- Query path = merge(daily snapshot, delta trie). Delta is bounded in size (last few hours).
- The delta is what most teams actually ship after launching the daily-batch baseline.

**"How would you personalize?"**

- Per-user history layer on top of the global suggestions.
- Re-rank the top-K returned by the trie using per-user signals (location, recent queries, language).
- The personalization model is the interesting ML problem; the system is "fetch global top-K, apply re-ranker."

**"How does this handle Chinese / Japanese?"**

- The prefix is *character* (or radical) instead of letter.
- Trie still works; nodes are keyed by characters.
- IME interactions get more complex — sometimes you're matching pinyin, sometimes characters.
- Build per-locale indices.

## What "senior" looks like vs. "mid-level"

- **Mid-level** picks a trie and stores suggestions in it.
- **Senior** precomputes top-K, separates batch from serving, names atomic snapshot reload, layers in a real-time delta when freshness demands it, and flags privacy concerns about leaking queries.
