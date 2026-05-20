# Week 15: Design Payments + Idempotency (Stripe)

## 🎯 The prompt

Design a payment processing service: API takes a charge request, talks to card networks, returns success/failure. Must be exactly-once even with network retries.

**Constraints the interviewer might give you:**

- 10k charges/sec at peak (Black Friday)
- < 500ms p99 charge latency
- Idempotent: retried charge with same key never double-charges
- Card network can return after timeout — we must not lose the result
- Must support multi-step flows (auth + capture, refunds, partial refunds)

**Suggested time budget:** 45 minutes

| Phase | Time |
|---|---|
| Clarify scope | 5 min |
| Idempotency + ledger architecture | 25 min |
| Card-network reconciliation deep dive | 15 min |

## ✅ Your job

1. **Try this yourself first.** 45-minute timer.
2. **Then open [answer.md](answer.md).**
3. **Read [interviewer-cues.md](interviewer-cues.md).**

## 💡 What you should already know

- Idempotency keys at the API layer
- Outbox pattern for reliable async work
- Saga vs. two-phase commit for distributed transactions
- Double-entry ledger basics
- PCI-DSS scope minimization
- The fact that real payment networks are *not* idempotent (they can return after a timeout)
