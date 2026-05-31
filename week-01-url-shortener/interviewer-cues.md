# Week 01: Interviewer cues — URL Shortener

This is the meta layer. None of it is in the answer file; all of it is what a senior interviewer is actually scoring against.

## What they're listening for

| Signal | What it means |
|---|---|
| You ask 2+ clarifying questions before drawing | "Stops to think, doesn't rush." |
| You name the read/write ratio early | "Sees the problem shape, not just the surface." |
| You sketch numbers (40 writes/sec → 400 reads/sec avg, ~4k peak) | "Comfortable with scale, doesn't hand-wave." |
| You explain *why* 302 not 301 | "Knows the protocol details." |
| You spontaneously flag the enumeration attack | "Thinks about security without being prompted." |
| You separate the read service from the write service | "Knows that read-heavy systems need independent scaling." |
| You name what you're *not* doing | "Disciplined about scope." |
| You finish a deep dive and say 'want me to go further?' | "Calibrates with the interviewer, doesn't run off." |

## Yellow flags

- Picking Cassandra or DynamoDB without a reason. (When asked why, "for scale" is not an answer.)
- Skipping back-of-envelope math.
- Drawing a microservices architecture for a problem that fits on a laptop.
- Not mentioning the CDN edge-cache opportunity — for *this* problem it's the single biggest scaling lever.

## Red flags

- Diving into implementation without clarifying scope.
- Claiming a design decision is "best practice" without naming the tradeoff it's making.
- Getting flustered by a follow-up question rather than treating it as input.
- Not asking how much time you have left.

## The killer follow-up

If you finish early, expect: **"What if I told you a single URL is getting 1M requests per second?"**

This is a flash-crowd / hot-key problem. The answer is:

1. CDN edge-cache the redirect — most requests never hit your service.
2. If they all *do* hit you, your single Redis node for that key becomes the bottleneck. Solve with client-side caching, request coalescing, or replicating the hot key across multiple cache nodes.
3. Acknowledge that at this scale you may need to switch from cache-aside to read-through with a smarter loader.

Saying "I'd CDN it" without elaborating is fine. Showing you know what comes *after* the CDN runs out is what gets the senior signal.

## What "senior" looks like vs. "mid-level"

- **Mid-level** describes *what* the system does.
- **Senior** describes *what could go wrong* and how the design *changes* under pressure.

Mid-level designs the happy path. Senior pre-empts the follow-up.
