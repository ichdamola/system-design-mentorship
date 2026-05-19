# Week 12: Interviewer cues — Live Comments

## What they're listening for

| Signal | What it means |
|---|---|
| You distinguish this from 1:1 chat | "Knows broadcast and conversation are different problems." |
| You name pub/sub as the fan-out mechanism | "Has the right tool." |
| You shard rooms above a viewer threshold | "Anticipates the million-viewer case." |
| You bring up backpressure (slow mode vs. sampling) | "Has thought about what breaks at scale." |
| You separate cheap inline moderation from async ML | "Respects the latency budget." |
| You bound the replay buffer | "Knows chat is ephemeral." |
| You flag reconnect storms when streams go live | "Has been on call." |

## Yellow flags

- One pub/sub topic for a million-viewer room.
- Persistent per-viewer chat history.
- Synchronous toxicity scoring on every message.
- Treating chat like email (with delivery receipts).
- "Just use Kafka" with no pub/sub semantics conversation.

## Red flags

- HTTP polling for chat.
- One database write per message per viewer.
- No moderation at all in the hot path.
- "We'll buffer all chat in Postgres and clients poll."
- Assuming linear scale (10× viewers = 10× cost — actually it's 100× because fan-out is multiplicative).

## The killer follow-up

**"What happens when a stream with 1M viewers gets a chat-rate spike — say a game-winning goal?"**

- Chat rate jumps from 100 msg/sec to 10k msg/sec for 30 seconds.
- At 1M viewers × 10k msg/sec = 10B deliveries/sec — undeliverable.
- Mitigations:
  - Slow mode activates automatically when room write rate crosses threshold.
  - Per-viewer client-side dedup if exact-once isn't needed (but we don't have dedup keys here).
  - Sample at the edge if absolutely necessary; acknowledge it's a degraded UX.
  - Most importantly: capacity planning assumed this peak. Edge fleet has 30% headroom.

**"How do you handle a banned user opening a new account?"**

- Out of scope for chat system; this is an identity / abuse problem.
- We honor the ban list as-given. The platform's anti-abuse system decides who's banned.
- Chat system might surface signals (e.g., "rapid creation of new accounts joining the same room") to that system.

**"Cross-region: what if half the viewers are in Europe and half in the US?"**

- Per-region pub/sub mesh, replicated across regions with async replication.
- ~100ms cross-region latency on messages.
- Acceptable for chat; some platforms run per-region rooms (separate conversations) for global events instead.

## What "senior" looks like vs. "mid-level"

- **Mid-level** picks pub/sub and a WebSocket layer.
- **Senior** shards rooms above a viewer threshold, picks slow mode over sampling, separates moderation latency tiers, and treats chat as ephemeral by design — not "we just haven't built persistence yet."
