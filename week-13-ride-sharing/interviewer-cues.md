# Week 13: Interviewer cues — Ride-Sharing

## What they're listening for

| Signal | What it means |
|---|---|
| You pick H3 / S2 / geohash and explain the cell-neighbor trick | "Knows real-world geospatial." |
| You shard the spatial index per region | "Notices the natural shard key." |
| You scope out payments / pricing as separate systems | "Doesn't try to design Uber in 45 minutes." |
| You make the spatial index in-memory + reconstructible | "Has thought about latency vs. durability." |
| You use parallel offers with first-to-accept resolution | "Knows the matching SLA matters." |
| You name SETNX or DB transactions for the race | "Has built atomic primitives in real systems." |
| You bring up reconnect storms on location update | "Has been on call." |

## Yellow flags

- PostGIS as the spatial backend (works at small scale, fails at 5M drivers).
- One-at-a-time offers.
- Storing every location update with same durability as rides themselves.
- Pretending global spatial index is needed.
- Forgetting the accept-race condition.

## Red flags

- "We'll query the database for nearby drivers" — at 1.25M location updates/sec, the database is on fire.
- No state machine for the ride lifecycle.
- "We'll just use a NoSQL store" without explaining how it answers "5 nearest."
- Treating ETA as a system-design problem we have to solve here (it's a routing service call).
- Synchronous payment in the ride completion flow.

## The killer follow-up

**"What about surge pricing?"**

- Per-cell demand/supply ratio computed in real time (rolling window).
- Price multiplier per cell, published.
- Riders see the multiplier in the fare estimate; can wait or accept.
- This is a *feedback loop* — high prices attract drivers, attract drivers reduce surge, etc.
- Acknowledged complexity: gaming (drivers logging off to trigger surge), regulation, fairness.

**"What about UberPool?"**

- Matching becomes graph optimization, not nearest-neighbor.
- Each potential pool has a route through multiple pickup/dropoff points.
- Score: total time vs. solo, total fare vs. solo, detour tolerance per rider.
- Reuses the geospatial index for candidate generation; adds a constraint solver for the assignment.
- Much harder problem; usually a separate service that calls this matching infra for candidates.

**"How do you handle a major event — football match with 100k riders dispatching all at once?"**

- Per-cell capacity planning.
- Surge pricing dampens demand (and signals drivers to come in).
- Pre-positioning drivers via incentives.
- Queue-based dispatch with longer wait times communicated to rider.
- Cross-cell expansion when local supply is exhausted.

## What "senior" looks like vs. "mid-level"

- **Mid-level** picks geohash and a database.
- **Senior** uses H3, shards per region, makes the spatial index in-memory + Kafka-backed, designs parallel offers with race-safe acceptance, and treats surge/pool/payments as separate systems that consume events from this one.
