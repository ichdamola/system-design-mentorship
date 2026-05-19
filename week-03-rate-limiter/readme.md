# Week 03: Design a Rate Limiter

## 🎯 The prompt

Design a rate limiter that sits in front of an HTTP API and rejects requests that exceed a configurable per-client quota (e.g. 100 requests per minute per API key).

**Constraints the interviewer might give you:**

- 1M requests per second across the fleet
- ~100k distinct API keys
- Limits configurable per key, per route
- 429 responses must include `Retry-After`
- Limiter cannot become a single point of failure

**Suggested time budget:** 45 minutes

| Phase | Time |
|---|---|
| Clarify scope | 5 min |
| Algorithm choice + architecture | 25 min |
| Distributed-state deep dive + tradeoffs | 15 min |

## ✅ Your job

1. **Try this yourself first.** Set a 45-minute timer.
2. **Then open [answer.md](answer.md).**
3. **Read [interviewer-cues.md](interviewer-cues.md).**

## 💡 What you should already know

- Token bucket, leaky bucket, fixed window, sliding window — the four classic algorithms
- Redis atomic operations (`INCR`, Lua scripts)
- Where in the request stack a limiter can live (edge vs. gateway vs. service)
- Lamport / logical clocks (helpful for sliding-window math, not required)
