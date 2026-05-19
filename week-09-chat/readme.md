# Week 09: Design a Chat System (WhatsApp / Messenger)

## 🎯 The prompt

Design a 1:1 and group chat service: a user sends a message, the system delivers it to recipients (including offline ones), and shows delivery and read receipts.

**Constraints the interviewer might give you:**

- 500M DAU
- 50 messages per user per day average
- Group chats up to 256 members
- Messages must persist for ≥30 days; clients can resync after going offline
- < 100ms p99 message delivery within a region

**Suggested time budget:** 45 minutes

| Phase | Time |
|---|---|
| Clarify scope | 5 min |
| Connection layer + storage architecture | 25 min |
| Group fan-out + offline delivery deep dive | 15 min |

## ✅ Your job

1. **Try this yourself first.** 45-minute timer.
2. **Then open [answer.md](answer.md).**
3. **Read [interviewer-cues.md](interviewer-cues.md).**

## 💡 What you should already know

- WebSocket vs. HTTP long-polling vs. XMPP (briefly)
- Per-conversation message ordering (sequence numbers)
- Idempotency keys
- Push notifications (APNs / FCM) for cold delivery
- Capped queues / sliding windows for offline replay
