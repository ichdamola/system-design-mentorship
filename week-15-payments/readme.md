# Week 15: Design Payments + Idempotency (Stripe)

## 🎯 The prompt

Design a payment processing service: API takes a charge request, talks to card networks, returns success/failure. Must be exactly-once even with network retries.

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

- Idempotency keys
- Two-phase commit vs. saga vs. outbox
- Reconciliation with external systems
- PCI scope and data isolation

---

> 🚧 **This week is scaffolded.** The 10-step skeleton is in place in
> [answer.md](answer.md); full content will be written out over time. See
> [Week 01](../week-01-url-shortener/) for the format your answer file will follow.
