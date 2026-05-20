# Week 16: Design a Distributed Search Engine (Elasticsearch)

## 🎯 The prompt

Design a distributed search engine that ingests documents, builds an inverted index across many nodes, and serves low-latency keyword + faceted queries.

**Constraints the interviewer might give you:**

- 100B documents indexed
- 100k queries per second
- Median document ~2 KB
- Indexing latency < 1 minute (near real-time)
- Faceted aggregations (e.g. group by category, time bucket) in the same query

**Suggested time budget:** 45 minutes

| Phase | Time |
|---|---|
| Clarify scope | 5 min |
| Inverted index + sharding | 25 min |
| Real-time indexing + query path deep dive | 15 min |

## ✅ Your job

1. **Try this yourself first.** 45-minute timer.
2. **Then open [answer.md](answer.md).**
3. **Read [interviewer-cues.md](interviewer-cues.md).**

## 💡 What you should already know

- Inverted index basics (term → list of doc IDs)
- Document partitioning (each shard holds a subset of docs) vs. term partitioning (each shard holds a subset of terms)
- Why nearly everyone uses document partitioning
- Replica shards for read throughput
- TF-IDF and BM25 (briefly — the scoring math)
- Lucene segments and how "near real-time" search works
