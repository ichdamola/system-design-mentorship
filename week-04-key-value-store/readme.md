# Week 04: Design Distributed Key-Value Store

## 🎯 The prompt

Design a distributed key-value store like Redis Cluster or DynamoDB — get/put/delete on string keys, sharded across many nodes, with replication for durability.

**Suggested time budget:** 45 minutes

| Phase | Time |
|---|---|
| Clarify scope | 5 min |
| Design + high-level architecture | 25 min |
| Deep dive + tradeoffs | 15 min |

## ✅ Your job

1. **Try this yourself first.** Set a 45-minute timer and work through the 10 steps without looking at the answer.
2. **Then open [answer.md](answer.md).** Compare your approach.
3. **Skim [interviewer-cues.md](interviewer-cues.md)** for what a senior interviewer is *really* listening for.

## 💡 What this week is really about

- Consistent hashing
- Replication (sync vs. async)
- Quorum reads/writes
- Hot-key problem

---

> 🚧 **This week is scaffolded.** The 10-step skeleton is in place in
> [answer.md](answer.md); full content will be written out over time. See
> [Week 01](../week-01-url-shortener/) for the format your answer file will follow.
