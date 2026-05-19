# Week 02: Interviewer cues — Pastebin

## What they're listening for

| Signal | What it means |
|---|---|
| You recognize this as "URL shortener + blob storage" | "Sees the shape of problems, not just the surface." |
| You separate metadata from content | "Knows what each storage layer is good at." |
| You bring up object stores (S3) unprompted | "Comfortable with the modern stack." |
| You explain *why* SQL is wrong for 1 MB blobs | "Understands the cost of mixing access patterns." |
| You mention CDN for the raw endpoint | "Thinks about read-path offload." |
| You flag the two-system expiration problem | "Notices that data has multiple lives." |
| You ask about abuse/scanning before being prompted | "Has shipped real products." |

## Yellow flags

- Storing content as `TEXT` in a SQL column without flagging the tradeoff.
- Treating this as a totally new problem instead of building on URL shortener.
- Designing for 10× more scale than the prompt called for.
- Not mentioning expiration cleanup at all.

## Red flags

- "I'd use MongoDB because it handles documents." — non-answer.
- Drawing a fan-out architecture with queues for a 4 writes/sec system.
- Skipping the metadata-vs-content split entirely.
- Suggesting full-text search on day one.

## The killer follow-up

**"What if a single paste goes viral and gets 1M requests per second?"**

Layered answer:

1. CDN cache on the raw endpoint, 5-minute TTL — most requests never reach your service.
2. S3 handles 3,500+ GET/sec per prefix; if you're worried, replicate the hot object to multiple keys.
3. Your service barely moves — it's metadata-only, and metadata fits in Redis.
4. The bottleneck shifts to the CDN's pricing tier, not your architecture.

**"What about abuse? Pastes get used to leak credentials."**

This is a real-world question, and senior candidates have an answer:

- Async scan pipeline (write to Kafka on paste creation, scanners pull and flag)
- Hash-based dedup against known-bad content (have-i-been-pwned style)
- Abuse report endpoint + manual review queue
- Soft delete first; hard delete after legal review

## What "senior" looks like vs. "mid-level"

- **Mid-level** designs a working pastebin.
- **Senior** flags abuse handling, expiration as a multi-system problem, and the CDN offload — without being asked.
