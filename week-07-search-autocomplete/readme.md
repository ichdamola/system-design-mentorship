# Week 07: Design Search Autocomplete

## 🎯 The prompt

Design the autocomplete suggestions that appear as a user types into a search bar (Google Suggest, YouTube search, etc.). Latency budget: ~50ms.

**Constraints the interviewer might give you:**

- 100B queries per day globally
- 10 character average query length
- Suggestions update daily (yesterday's popular queries inform today's suggestions)
- Personalized per user (favor their language, location, history)
- 99.9% availability

**Suggested time budget:** 45 minutes

| Phase | Time |
|---|---|
| Clarify scope | 5 min |
| Trie + Top-K design | 25 min |
| Distributed indexing + tradeoffs | 15 min |

## ✅ Your job

1. **Try this yourself first.** 45-minute timer.
2. **Then open [answer.md](answer.md).**
3. **Read [interviewer-cues.md](interviewer-cues.md).**

## 💡 What you should already know

- Trie data structure and how prefix queries work on it
- Top-K problems (heap-based selection)
- The difference between batch indexing and real-time indexing
- Locality / geographic deployment
- N-grams (briefly — not the canonical answer but worth knowing as an alternative)
