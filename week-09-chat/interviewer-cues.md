# Week 09: Interviewer cues — Chat System

## What they're listening for

| Signal | What it means |
|---|---|
| You pick WebSocket over polling without hand-waving | "Knows the mobile-battery cost of polling." |
| You name 50M concurrent connections as the constraint | "Sees the right axis of scale." |
| You separate the connection layer from the storage layer | "Knows you can't put all state in one place." |
| You bring up per-conversation sequence numbers | "Knows that *some* ordering matters and *all* ordering doesn't." |
| You mention idempotency keys on send | "Has shipped mobile clients." |
| You distinguish inbox queue from message store | "Understands the durability vs. delivery split." |
| You bring up reconnect storms as a failure mode | "Has been on the on-call rotation." |

## Yellow flags

- "We'll just use WebSocket" — and stopping there.
- One Kafka partition per user (50M partitions is unworkable).
- Putting `last_read_seq` on the message itself instead of the membership.
- Forgetting that delivered/read are separate states.
- Treating group fan-out as a "big enough to need celebrity logic" — it isn't, but conflating it with feed problems is a red flag.

## Red flags

- HTTP polling as the primary transport.
- One global message sequence across all conversations.
- "Messages live in Redis" with no durable backing store.
- No mention of what happens when a recipient is offline.
- Synchronous fan-out blocking the sender's API call.

## The killer follow-up

**"What about end-to-end encryption?"**

- Messages encrypted client-side; server stores ciphertext only.
- Each pair of devices runs a key-exchange (Signal: X3DH) and a ratchet (Double Ratchet) for forward secrecy.
- Server can't read or fan-out by content — only by recipient ID.
- Server can still implement read receipts and group membership; those leak metadata, not content.
- This is a *huge* change to operations (server can't search/moderate; we lose abuse mitigation tools).

**"What about multi-device — same user on phone + desktop?"**

- Per-device inbox queue.
- Each device acks separately. A message is "delivered to user" when any device acks.
- "Read" can be per-device or unified — product choice.
- E2E gets harder: each device is its own ratchet endpoint; one sender encrypts to N recipient devices.

**"How do you avoid duplicate messages on retry?"**

- Client supplies a `client_id` (UUID). Server dedups by `(conv_id, client_id)` unique index.
- If the same client_id is sent twice, the second is a no-op (return the original message_id).
- Without this, network retries create ghost messages.

## What "senior" looks like vs. "mid-level"

- **Mid-level** picks WebSocket and a database.
- **Senior** sizes for *connections* not QPS, separates connection from storage from queue, names idempotency keys and reconnect storms, treats E2E and multi-device as additive complexity to call out, not pretend to solve in 45 minutes.
