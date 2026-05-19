# Week 05: Design a News Feed (Facebook-style)

## 🎯 The prompt

Design a personalized news feed that shows a user the recent posts from people they follow, ordered by relevance (or recency).

**Constraints the interviewer might give you:**

- 1B users, 200M DAU
- Average user follows ~300 accounts
- 100M posts created per day
- Feed reads ≫ writes (10:1)
- Feed must load in < 300ms

**Suggested time budget:** 45 minutes

| Phase | Time |
|---|---|
| Clarify scope | 5 min |
| Push vs. pull architecture | 25 min |
| Hybrid fan-out + ranking deep dive | 15 min |

## ✅ Your job

1. **Try this yourself first.** 45-minute timer.
2. **Then open [answer.md](answer.md).**
3. **Read [interviewer-cues.md](interviewer-cues.md).**

## 💡 What you should already know

- The push (fan-out on write) vs. pull (fan-out on read) tradeoff
- Why neither pure approach works at Facebook scale
- The "celebrity problem"
- Redis sorted sets and capped lists
- Ranking pipelines (feature store → scoring → re-ranking)
