# Week 01: Design a URL Shortener (TinyURL)

## 🎯 The prompt

Design a service that lets users submit a long URL and get back a short URL (like `https://sho.rt/3xY7zPq`) that redirects to the original.

**Constraints the interviewer might give you:**

- 100M new URLs per month
- 10× read traffic vs. writes
- Short codes must not be guessable
- 5-year retention

**Suggested time budget:** 45 minutes total

| Phase | Time |
|---|---|
| Clarify scope | 5 min |
| Design + high-level architecture | 25 min |
| Deep dive + tradeoffs | 15 min |

## ✅ Your job

1. **Try this yourself first.** Get a piece of paper, set a 45-minute timer, work through the 10 steps without looking at the answer.
2. **Then open [answer.md](answer.md).** Compare your approach to the canonical walkthrough.
3. **Read [interviewer-cues.md](interviewer-cues.md)** to see what a senior interviewer is actually listening for — most of which won't be in any book.

## 💡 What you should already know

- HTTP basics (GET, POST, status codes, 301 vs 302)
- Database basics (SQL vs. NoSQL tradeoffs)
- Hash functions and the collision tradeoff
- Base62 encoding (`a–z A–Z 0–9`, 62 chars)
- What a CDN is and what it's good for
