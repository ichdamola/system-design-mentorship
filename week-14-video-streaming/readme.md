# Week 14: Design Video Streaming (YouTube)

## 🎯 The prompt

Design YouTube's video playback path: upload, transcode to multiple bitrates, store, and serve adaptive-bitrate streams to billions of viewers globally.

**Constraints the interviewer might give you:**

- 500 hours of video uploaded per minute
- 5B viewing hours per day
- 95% of views are from the top 5% of videos (long-tail catalog)
- Global audience; must work on 3G in emerging markets
- Live + VOD (we can scope to VOD if asked)

**Suggested time budget:** 45 minutes

| Phase | Time |
|---|---|
| Clarify scope | 5 min |
| Upload + transcode pipeline | 25 min |
| Serving + CDN strategy deep dive | 15 min |

## ✅ Your job

1. **Try this yourself first.** 45-minute timer.
2. **Then open [answer.md](answer.md).**
3. **Read [interviewer-cues.md](interviewer-cues.md).**

## 💡 What you should already know

- Adaptive bitrate (ABR) — HLS, DASH
- Encoding ladders (240p, 360p, 480p, 720p, 1080p, 4K)
- The role of a CDN in video delivery
- Why videos are stored as chunks, not single files
- Cold vs. hot content tiering
