# Week 08: Design a Web Crawler

## 🎯 The prompt

Design a polite, scalable web crawler that fetches billions of pages, respects robots.txt, deduplicates content, and feeds a downstream index (search, RAG, archive).

**Constraints the interviewer might give you:**

- 1B pages to crawl
- Target completion: 30 days
- Respect robots.txt and per-domain politeness
- Detect and skip duplicate / near-duplicate content
- Storage of fetched content: ~5 TB

**Suggested time budget:** 45 minutes

| Phase | Time |
|---|---|
| Clarify scope | 5 min |
| URL frontier + worker fleet | 25 min |
| Politeness + deduplication deep dive | 15 min |

## ✅ Your job

1. **Try this yourself first.** 45-minute timer.
2. **Then open [answer.md](answer.md).**
3. **Read [interviewer-cues.md](interviewer-cues.md).**

## 💡 What you should already know

- The classic "URL frontier" abstraction
- DNS caching and why crawl performance is often DNS-bound
- robots.txt and crawl-delay
- SimHash / shingling for near-duplicate detection
- The difference between depth-first and breadth-first crawl ordering
