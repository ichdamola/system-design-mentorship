# Week 04: Design a Distributed Key-Value Store

## 🎯 The prompt

Design a distributed key-value store like Redis Cluster or DynamoDB — `get(key)`, `put(key, value)`, `delete(key)` on string keys, sharded across many nodes, with replication for durability.

**Constraints the interviewer might give you:**

- 10 PB of data total
- 100k operations per second sustained
- Values up to 1 MB
- Multi-region replication
- 99.99% availability target

**Suggested time budget:** 45 minutes

| Phase | Time |
|---|---|
| Clarify scope | 5 min |
| Sharding + replication architecture | 25 min |
| Failure-mode deep dive + tradeoffs | 15 min |

## ✅ Your job

1. **Try this yourself first.** Set a 45-minute timer.
2. **Then open [answer.md](answer.md).**
3. **Read [interviewer-cues.md](interviewer-cues.md).**

## 💡 What you should already know

- Consistent hashing (and why it beats `hash(key) % N`)
- Quorum reads/writes (R + W > N → strong consistency)
- The CAP theorem (and its limits — you can't pick one and forget the others)
- The Dynamo paper and the Cassandra paper, at least in outline
- Vector clocks vs. last-write-wins
- Anti-entropy: Merkle trees, hinted handoff, read repair

## 📖 Helpful reading before you attempt

- *Dynamo: Amazon's Highly Available Key-Value Store* (the original paper)
- Cassandra's architecture docs
- Kleppmann, *DDIA*, chapters 5-6
