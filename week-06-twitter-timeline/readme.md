# Week 06: Design the Twitter Home Timeline

## 🎯 The prompt

Design Twitter's home timeline — the list of recent tweets from accounts a user follows, plus injected promoted content. This is the highest-scale read problem on the consumer internet.

**Constraints the interviewer might give you:**

- 400M MAU, 200M DAU
- 300M tweets per day
- p99 timeline load < 200ms
- Top accounts have 100M+ followers
- Timeline must include a mix of "for you" (ranked) and "latest" (chronological)

**Suggested time budget:** 45 minutes

| Phase | Time |
|---|---|
| Clarify scope | 5 min |
| Fan-out architecture (push, pull, hybrid) | 25 min |
| Celebrity problem deep dive + tradeoffs | 15 min |

## ✅ Your job

1. **Try this yourself first.** 45-minute timer.
2. **Then open [answer.md](answer.md).**
3. **Read [interviewer-cues.md](interviewer-cues.md).**

## 💡 What you should already know

- [Week 05 — News Feed](../week-05-news-feed/) — Twitter is the canonical version
- Redis sorted sets and pipelining
- The push-on-write architecture that Twitter publicly described in 2013
- Why pure push fails at the top end and pure pull fails at the bottom
- Snowflake IDs and time-sortable identifiers
