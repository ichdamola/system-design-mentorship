# Week 10: Design a Notification System

## 🎯 The prompt

Design a service that sends notifications (push, email, SMS) to users — triggered by application events, with deduplication, batching, and per-user preferences.

**Constraints the interviewer might give you:**

- 100M notifications per day across all channels
- ~70% push, 20% email, 10% SMS
- Per-user preferences (quiet hours, channel selection, opt-outs)
- Idempotent: same event triggered twice must send one notification
- Multi-provider per channel (FCM + APNS for push, multiple SMS vendors for failover)

**Suggested time budget:** 45 minutes

| Phase | Time |
|---|---|
| Clarify scope | 5 min |
| Multi-channel pipeline | 25 min |
| Idempotency + provider failover deep dive | 15 min |

## ✅ Your job

1. **Try this yourself first.** 45-minute timer.
2. **Then open [answer.md](answer.md).**
3. **Read [interviewer-cues.md](interviewer-cues.md).**

## 💡 What you should already know

- Idempotency keys
- Outbox pattern (vs. dual-write)
- Channel abstractions (push, email, SMS as plug-ins)
- Backpressure on downstream providers
- The concept of a quiet-hours window
