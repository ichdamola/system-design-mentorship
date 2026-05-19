# Week 04: Interviewer cues — Distributed Key-Value Store

## What they're listening for

| Signal | What it means |
|---|---|
| You explicitly pick AP or CP and explain why | "Has read the CAP paper, not just the title." |
| You draw the consistent-hash ring | "Knows the canonical design." |
| You mention virtual nodes | "Has actually built/operated one." |
| You write `R + W > N` for strong consistency | "Knows the quorum math." |
| You bring up hinted handoff *and* read repair *and* anti-entropy | "Knows that 'eventual' needs a mechanism." |
| You name tombstones and the GC grace period | "Has been bitten by ghost values." |
| You flag the hot-key problem before being asked | "Has shipped at scale." |
| You distinguish LWW from vector clocks (and pick one) | "Understands conflict resolution as a real choice." |

## Yellow flags

- "Use consistent hashing" without explaining what problem it solves.
- "It's eventually consistent" with no mechanism behind the word "eventually."
- "We'll just use Cassandra/Dynamo" — that's the answer, not the design.
- Picking N=2 W=1 R=1 without realizing it doesn't survive any node loss.
- Skipping the conflict-resolution conversation entirely.

## Red flags

- Drawing a master + slaves architecture and calling it Dynamo-style.
- Using `hash(key) % N` for sharding without flagging the rebalance cost.
- Claiming "strong consistency" without R+W>N.
- "I'd use a single Redis instance" for 10 PB.

## The killer follow-up

**"What happens during a network partition?"**

This tests whether you really understand CAP, not just the acronym.

- AP design (what we built): writes continue on both sides of the partition. After the heal, replicas converge via read repair / anti-entropy. Conflicting writes are resolved (LWW or app-level).
- CP design (e.g. etcd, Spanner with TrueTime): the minority side stops accepting writes. The majority stays consistent. Customers see partial unavailability.
- Pick the one that matches what your customer needs. **Don't dodge by saying "we use Paxos so we're fine."**

**"How does cross-region replication work?"**

- Async by default — synchronous cross-region kills your latency budget.
- Per-region quorums (writes succeed locally, replicate to other regions in the background).
- Conflicts resolved with LWW + hybrid logical clocks (HLC).
- If you need globally-consistent writes (rare), Spanner-style TrueTime is the only honest answer.

## What "senior" looks like vs. "mid-level"

- **Mid-level** reproduces the Dynamo diagram.
- **Senior** explains *why* AP for general KV, *why* CP for coordination, *why* tombstones, *why* hinted handoff is not enough on its own — and names the hot-key problem as the actual production headache.
