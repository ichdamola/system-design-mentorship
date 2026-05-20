# Week 16: Interviewer cues — Distributed Search

## What they're listening for

| Signal | What it means |
|---|---|
| You pick document partitioning and explain why | "Knows what Elasticsearch actually does." |
| You name Lucene-style segments | "Has thought about how writes don't block reads." |
| You scatter-gather queries with hedging | "Has shipped a production search service." |
| You treat the index as reconstructible | "Knows the right durability boundary." |
| You distinguish keyword from vector search | "Doesn't conflate modern semantic with classical." |
| You bring up tail-latency mitigation | "Has been on call." |
| You mention near-real-time as a refresh interval | "Knows the underlying mechanism." |

## Yellow flags

- Term partitioning without acknowledging why it's not used.
- Mutable, in-place index updates.
- "We'll just use Elasticsearch" without explaining the design underneath.
- Synchronous replication assumed for indexing.
- No mention of how segments / compaction work.

## Red flags

- One global shard.
- Treating the search index as the source of truth.
- "We'll index synchronously on write" with no buffer / segment / batch.
- No replication at all.
- Suggesting a SQL database with `LIKE '%query%'` for full-text search.

## The killer follow-up

**"How would you support vector / semantic search?"**

- Different data structure: HNSW or IVF instead of inverted index.
- Documents get embedded (model in front), vectors stored.
- Query: embed → ANN lookup.
- Often combined with keyword: hybrid scoring (BM25 + cosine).
- Acknowledge: that's the same cluster topology but parallel index structures.

**"What if one shard becomes very hot — say 10× the queries of others?"**

- Diagnose: is it data skew, or query skew?
- Data skew: custom routing key; rebalance via reindex.
- Query skew: scale up replicas of the hot shard; query routing weighted to balance load.
- For a single document drawing all attention: read-through cache in front of the cluster.

**"What about cross-cluster search?"**

- Federated query: dispatcher fans out to each cluster, gathers, merges.
- Each cluster runs independent; coordinator only at the federation layer.
- Used when data sovereignty / locality forces it.
- Tradeoff: doubled scatter-gather; tail latency compounds.

**"Why not just use Postgres full-text search?"**

- Postgres FTS is great up to ~10M docs / single node.
- Loses at 100B docs scale (no partitioning).
- Lacks distributed scatter-gather.
- Aggregations / facets less developed.
- Sometimes the right call for small projects; not for the problem we were given.

## What "senior" looks like vs. "mid-level"

- **Mid-level** picks "use Elasticsearch."
- **Senior** explains *why* document partitioning, *why* Lucene segments, *why* hedged scatter-gather, names tail-latency mitigations, and treats vector search as a sibling design rather than pretending one structure does both.
