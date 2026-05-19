# Week 02: Design a Pastebin

## 🎯 The prompt

Design a service where users paste text (code snippets, error logs, configs) and get back a short URL anyone can visit to view the content. Think pastebin.com or gist.github.com.

**Constraints the interviewer might give you:**

- 10M new pastes per month
- Average paste size: 10 KB; max: 1 MB
- 5:1 read/write ratio
- Pastes can be public, unlisted, or set to expire

**Suggested time budget:** 45 minutes

| Phase | Time |
|---|---|
| Clarify scope | 5 min |
| Design + high-level architecture | 25 min |
| Deep dive + tradeoffs | 15 min |

## ✅ Your job

1. **Try this yourself first.** Set a 45-minute timer.
2. **Then open [answer.md](answer.md).** Compare your approach.
3. **Read [interviewer-cues.md](interviewer-cues.md)** for the senior-vs-mid signal.

## 💡 What you should already know

- HTTP basics
- Object storage (S3-style blob stores) and pre-signed URLs
- TTL / expiration patterns
- The URL shortener problem ([Week 01](../week-01-url-shortener/)) — this builds on it
- Content vs. metadata separation
