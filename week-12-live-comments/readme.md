# Week 12: Design Live Comments (Twitch chat)

## 🎯 The prompt

Design live chat for a streaming platform — thousands of viewers in one room, messages must broadcast in real time with low latency, and moderation has to keep up.

**Constraints the interviewer might give you:**

- 100k concurrent streams
- Avg 1k viewers / stream; top streams 1M+
- Median chat rate 10 messages/min; top streams 1000+ msgs/sec in one room
- p99 delivery latency < 500ms in-room
- Late joiners see the last N messages (replay)

**Suggested time budget:** 45 minutes

| Phase | Time |
|---|---|
| Clarify scope | 5 min |
| Pub/sub fan-out + room sharding | 25 min |
| Backpressure + moderation deep dive | 15 min |

## ✅ Your job

1. **Try this yourself first.** 45-minute timer.
2. **Then open [answer.md](answer.md).**
3. **Read [interviewer-cues.md](interviewer-cues.md).**

## 💡 What you should already know

- Pub/sub (Redis pub/sub, Kafka, NATS) — when to pick each
- The difference between chat ([Week 09](../week-09-chat/)) and broadcast chat (this week)
- Backpressure strategies (drop, sample, slow-mode)
- Sharding a single "room" across multiple servers when one server can't hold all viewers
- Why you can't use chat-style 1:1 persistence here
