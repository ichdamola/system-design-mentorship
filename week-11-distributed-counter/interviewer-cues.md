# Week 11: Interviewer cues — Distributed Counter

## What they're listening for

| Signal | What it means |
|---|---|
| You ask "exact or approximate?" first | "Knows the cost difference is enormous." |
| You sharded counters before being asked | "Anticipates the hot-key problem." |
| You name lambda architecture (speed + batch) | "Knows that streaming alone leaves you exposed." |
| You bring up HyperLogLog for unique counts | "Has the right tool in the kit." |
| You CDN-cache the read path | "Reflexive about read offload." |
| You tier the sharding (cold/warm/hot) | "Doesn't over-engineer for the long tail." |
| You make batch authoritative over speed | "Knows which one is right when they disagree." |

## Yellow flags

- One counter row per video.
- Designing only the write path (forgetting the read demolishes the system).
- Treating "exact" as a default requirement.
- No reconciliation pipeline.
- Pure streaming with no batch backup.

## Red flags

- Synchronous `UPDATE counter SET count = count + 1` at 5M/sec.
- Per-event SQL transaction.
- "We'll just use Redis INCR" with no sharding for hot keys.
- Reading the counter directly from a database on page load.
- "Eventually consistent" as a substitute for designing the consistency.

## The killer follow-up

**"What happens if a video goes from 10 views to 100M views in an hour?"**

This is the *promotion* problem.

- Initial state: single counter row, fine.
- Monitor detects rising write rate.
- Promote to 16-shard counter atomically:
  - Create the 15 new shard rows.
  - Update routing config (events now hash across all 16).
  - The existing row's value lives in shard 0; new events spread across 0-15.
- The promotion itself is the interesting design — it has to be online (can't pause writes) and consistent (no double counting during the swap).

**"How do you handle a network partition between speed and batch layers?"**

- The data lake (S3) is the durable source. As long as event ingestion to the lake works, batch is fine.
- The speed layer is best-effort. If it falls behind during a partition, the next batch run corrects it.
- The display might be stale during the partition. Acceptable.

**"What if the batch layer disagrees with the speed layer by 10%?"**

- Batch wins. Overwrite speed-layer counters with batch values.
- Investigate why speed drifted that far — usually a bug in the streaming job, dropped events, or a partition-skew issue.
- Alert on disagreement above a threshold; that's a known-bad signal worth paging.

## What "senior" looks like vs. "mid-level"

- **Mid-level** uses Redis INCR.
- **Senior** asks exact vs. approximate first, tiers the sharding strategy, uses lambda with batch as the source of truth, and treats display caching as part of the design — not an afterthought.
