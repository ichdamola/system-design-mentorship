# Week 03: Design Rate Limiter

## 🎯 The prompt

Design a rate limiter that sits in front of an HTTP API and rejects requests that exceed a configurable per-client quota (e.g. 100 req/min per API key).

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

- Token bucket vs. leaky bucket vs. fixed window vs. sliding window
- Distributed counter problem
- Where the limiter lives (edge vs. service)

---

> 🚧 **This week is scaffolded.** The 10-step skeleton is in place in
> [answer.md](answer.md); full content will be written out over time. See
> [Week 01](../week-01-url-shortener/) for the format your answer file will follow.
