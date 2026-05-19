# Week 05: Interviewer cues — News Feed

## What they're listening for

| Signal | What it means |
|---|---|
| You name push vs. pull early and contrast them | "Knows the canonical tradeoff." |
| You arrive at hybrid fan-out unprompted | "Has read about how Facebook actually does it." |
| You quantify the celebrity problem (1 post × 100M followers) | "Comfortable with the math, not just the words." |
| You separate ranking from candidate generation | "Has built or seen a real feed pipeline." |
| You bring up Kafka / event-driven fan-out | "Knows that synchronous fan-out doesn't work." |
| You mention sorted sets and capping the timeline | "Has actually built one in Redis." |
| You flag edit/delete propagation | "Thinks past the happy path." |

## Yellow flags

- "I'd push to all followers" — fine for small scale; bad if you can't acknowledge it breaks at celebrity scale.
- Doing fan-out *synchronously* in the post API.
- Storing the full post object in every timeline cache (should be post_id only).
- Treating ranking as a system-design problem you have to design.
- Not capping the cached timeline length.

## Red flags

- Suggesting that every read should query "the database" for the timeline — that's 300 follower JOINs.
- Saying the system is "real-time" without quantifying.
- Drawing a diagram with no Kafka/queue/event-bus and saying "the post service calls the feed service directly."
- Not noticing that 100M followers × 1B users gets you to terabytes of timeline cache.

## The killer follow-up

**"What about edits and deletes?"**

The cache-invalidation problem.

- Edits to a post: don't rewrite timelines — store the post by `post_id` in a `posts` table; timelines only hold IDs; reads dereference. Edits show automatically.
- Deletes: trickier — timelines still have the deleted post_id. Lazy: read service checks `is_deleted` flag and filters. Eager: tombstone in the timeline cache via fan-out.
- Most production systems do lazy invalidation and accept brief "ghost posts" in timeline order.

**"How would you handle ranking at this scale?"**

- Candidate generation is the systems problem (cap at ~500).
- Feature serving from a feature store with low latency.
- Two-stage: cheap heuristic model for everyone, expensive ML for engaged users.
- Re-ranking pass for diversity + business rules + ads (a totally separate pipeline).

**"How do you guarantee a post reaches every follower's feed?"**

You don't, strictly. You design for eventual consistency. Kafka with replication gives you at-least-once delivery. Fan-out workers are idempotent (set semantics on the sorted set). Lost writes get caught by anti-entropy / repair jobs that compare the timeline cache against the source of truth.

## What "senior" looks like vs. "mid-level"

- **Mid-level** picks push or pull.
- **Senior** picks hybrid, explains the threshold as a knob, breaks ranking out as its own pipeline, and flags edit/delete as the production headache nobody warns you about.
