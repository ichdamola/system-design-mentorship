# Week 03: Interviewer cues — Rate Limiter

## What they're listening for

| Signal | What it means |
|---|---|
| You ask "where in the stack" before designing | "Knows there's no one right answer." |
| You compare ≥3 algorithms with tradeoffs | "Has actually built one." |
| You pick token bucket and can defend it | "Stripe/Cloudflare/AWS-style thinking." |
| You write the Lua snippet or describe atomicity | "Knows you can't INCR + check separately." |
| You explicitly choose fail-open or fail-closed | "Thinks about failure modes." |
| You mention per-route / per-endpoint costs | "Has shipped a real API." |
| You bring up `Retry-After` and client backoff | "Thinks about both sides of the contract." |

## Yellow flags

- Saying "I'd use a sliding window" without naming which variant.
- Designing local-only counters without acknowledging the multi-instance problem.
- Forgetting the response should include `Retry-After`.
- Treating Redis as a regular key-value store and missing the atomicity issue.
- Not mentioning what happens when Redis is down.

## Red flags

- Suggesting an in-memory limiter without the multi-instance fix.
- Picking leaky bucket for HTTP rate limiting (queueing-based, wrong semantics).
- Drawing a database table to store per-request timestamps for "sliding window."
- Saying "we'll just scale Redis" without explaining how.

## The killer follow-up

**"What if a single API key is sending 1M req/sec?"**

This is the hot-key problem.

1. Your one Redis shard for that key becomes the bottleneck.
2. Solution 1: split the limit across N virtual buckets and route by request hash; the client effectively gets 1/N of the rate per bucket. Approximate but cheap.
3. Solution 2: pre-allocate token budgets to each gateway instance (e.g. each gateway gets 10% of the per-second rate every 100ms). Drastically reduces Redis calls; small temporary over/under shoot is acceptable.
4. In practice, customers hitting this kind of rate usually have a contract — they're not "rate limited," they're "throttled to their tier."

**"How would you globally rate-limit across regions?"**

- Don't, if you can avoid it. Per-region limiting is usually enough and avoids cross-region RTT on the hot path.
- If you must: aggregate counters async to a global state every N seconds; regions self-throttle when the global counter approaches the limit. Accept that the global limit is approximate.

## What "senior" looks like vs. "mid-level"

- **Mid-level** picks an algorithm and a data store.
- **Senior** chooses the failure mode (fail-open vs. fail-closed) explicitly, batches the token consumption to cut Redis load, and knows that per-region limiting is enough for 99% of products.
