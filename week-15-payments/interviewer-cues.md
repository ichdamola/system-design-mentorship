# Week 15: Interviewer cues — Payments + Idempotency

## What they're listening for

| Signal | What it means |
|---|---|
| You make idempotency key required at the API | "Treats correctness as non-negotiable." |
| You enforce idempotency at the database layer | "Knows app-level checks have race conditions." |
| You design `in_doubt` as a real state, not an error | "Has built financial systems." |
| You bring up reconciliation against the network ledger | "Knows the network is the source of truth, not us." |
| You use outbox pattern for webhooks | "Has solved dual-write reliably before." |
| You separate event log from projected state | "Audit-aware." |
| You explicitly say 'don't auto-retry the network' | "Understands the double-charge failure mode." |

## Yellow flags

- App-level idempotency check (`if seen[k]: return cached`).
- Auto-retry on card-network timeout.
- Single charges table updated in place (no audit trail).
- Webhook delivery in the charge API path.
- "We'll use distributed transactions across services."

## Red flags

- Idempotency as optional.
- "We'll just trust the card network's response" — but how, if you didn't get one?
- Charging the customer before persisting intent.
- No reconciliation pipeline.
- No mention of PCI scope or tokens.

## The killer follow-up

**"What happens when your call to the card network times out?"**

- You don't know if the charge succeeded.
- Two wrong answers: (a) retry — risks double-charge, (b) mark failed — risks losing a real charge.
- Correct: mark `in_doubt`. Return a status to the merchant indicating not-yet-resolved. Don't auto-retry.
- Reconciliation job the next day reads the network's settlement file and resolves the state.
- During the window, merchants retrying with the same idempotency key get a 409 with `Retry-After` (or a "check status" hint).

**"What if a merchant sends the same idempotency key with a different body?"**

- 409 Conflict. Don't process it.
- Stripe's published behavior. Surface a clear error so the merchant knows their key is reused.

**"How do you handle a card-network outage?"**

- Per-network circuit breaker. If Visa is failing at 50%, fast-fail Visa charges with a clear error (`network_unavailable`).
- Mastercard / other networks continue normally.
- Alert the on-call.
- Don't queue charges for later — payment data going stale is a separate set of problems.

**"How do you ensure the ledger never loses money?"**

- Append-only event log.
- Double-entry bookkeeping at the accounting layer (every debit has a credit).
- Daily reconciliation: sum(events in our log) == sum(transactions in network's settlement file).
- Disagreement triggers a page — money never silently disappears.

## What "senior" looks like vs. "mid-level"

- **Mid-level** designs a charge API.
- **Senior** centerpieces idempotency, designs `in_doubt` as a real state with reconciliation, uses outbox for webhooks, and treats the card network as a non-cooperative external system rather than a regular service.
