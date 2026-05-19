# Week 10: Interviewer cues — Notification System

## What they're listening for

| Signal | What it means |
|---|---|
| You enforce idempotency at the database, not in app code | "Knows race conditions are unavoidable in distributed systems." |
| You name circuit breakers per provider | "Has done a real failover design." |
| You separate transactional from marketing | "Has been paged when a marketing surge starved a 2FA flow." |
| You bring up quiet hours in user's timezone | "Thinks about user experience, not just architecture." |
| You distinguish event_id (input) from notification_id (output) | "Has thought about traceability." |
| You include `category` and a preference layer | "Recognizes that 'send everything' is not the product." |
| You mention exponential backoff with a cap | "Knows retry storms break things." |

## Yellow flags

- Application-level idempotency (`if not seen: send; seen.add(...)`).
- One provider per channel.
- Transactional and marketing on the same queue.
- Synchronous trigger → provider call.
- No mention of preferences or opt-outs.

## Red flags

- "We'll check Redis to see if we already sent it" — race condition.
- Designing this as if every notification is transactional and instant.
- No retry strategy whatsoever.
- Storing FCM/APNS tokens but never rotating them.
- Ignoring quiet hours.

## The killer follow-up

**"The CEO sends a 'thank you' notification to all 100M users at 9am. What happens?"**

- This is a 100M notification surge in seconds. Without protection:
  - Preferences store gets flattened.
  - Dispatcher fleet runs out of capacity.
  - Twilio rate-limits us.
  - Transactional flow stalls.
- With protection:
  - Marketing campaign is on its own topic/queue.
  - Dispatcher rate-limited to ~10× normal throughput.
  - Spread over hours; per-user `deliver_at` distributed across the window.
  - Transactional lane is on different infrastructure; never affected.

**"What about cross-channel suppression?"**

- Don't send the same alert via push AND email AND SMS within 60 seconds.
- Mark a `(user, category, time_window)` key when any channel succeeds; subsequent channels short-circuit.
- Some categories override (2FA must hit all configured channels regardless).

**"What about user device updating?"**

- Tokens expire. FCM/APNS return errors on stale tokens.
- Dispatcher records the failure, marks the token invalid.
- Client re-registers on next launch.
- Don't retry to a token that's already known-bad.

## What "senior" looks like vs. "mid-level"

- **Mid-level** designs a pipeline.
- **Senior** centerpieces idempotency, names circuit breakers per provider, separates marketing from transactional lanes, and respects user preferences as a first-class data model (not an afterthought).
