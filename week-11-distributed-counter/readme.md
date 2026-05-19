# Week 11: Design a Distributed Counter (YouTube views)

## 🎯 The prompt

Design the system that counts video views on YouTube — billions of increment events per day, eventual consistency is fine, but views must be "roughly" accurate within seconds for the displayed count.

**Constraints the interviewer might give you:**

- 5B videos in catalog
- 50B view events per day (avg 10 views/video; long tail)
- Top videos: 10M+ views per hour
- Display latency: counter on the video page must be < 1s stale
- Reads ≫ writes (every page load reads; only viewers write)

**Suggested time budget:** 45 minutes

| Phase | Time |
|---|---|
| Clarify scope | 5 min |
| Sharded counter + aggregation pipeline | 25 min |
| Hot-key + accuracy tradeoffs | 15 min |

## ✅ Your job

1. **Try this yourself first.** 45-minute timer.
2. **Then open [answer.md](answer.md).**
3. **Read [interviewer-cues.md](interviewer-cues.md).**

## 💡 What you should already know

- The hot-key problem (one key getting all the traffic)
- Sharded counters and how to combine them on read
- Lambda architecture (batch + streaming)
- HyperLogLog for approximate unique counts
- The difference between exact and approximate counting (and why YouTube doesn't care about exact)
