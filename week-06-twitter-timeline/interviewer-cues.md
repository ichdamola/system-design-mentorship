# Week 06: Interviewer cues — Twitter Home Timeline

## What they're listening for

| Signal | What it means |
|---|---|
| You name the celebrity problem with a real number (100M followers) | "Comfortable with the actual scale." |
| You arrive at hybrid fan-out without prompting | "Has read about Twitter's architecture." |
| You use a shared per-celebrity cache, not per-follower | "Knows the optimization that makes this affordable." |
| You mention Snowflake / time-sortable IDs | "Has thought about cursor pagination." |
| You bring up Kafka or an event log | "Knows you can't do this synchronously." |
| You separate "for you" from "latest" | "Knows the ranked timeline is a different problem." |
| You flag two-tier fan-out (push vs. delayed push vs. pull) | "Goes one level deeper than the standard answer." |

## Yellow flags

- Storing the full tweet object in every timeline cache (instead of just IDs).
- Doing fan-out inside the POST /tweets API.
- Picking a celebrity threshold without explaining what it's optimizing.
- Designing the ranked timeline as a separate parallel system instead of layering on this one.
- Not naming Snowflake or any time-sortable ID scheme.

## Red flags

- "Just use Cassandra" — for what?
- "I'd query the database for the timeline" — 700 follower JOINs, p99 5s.
- Drawing a single Redis instance for 200M users.
- Saying "real-time" without quantifying.
- Suggesting two-phase commit anywhere in this design.

## The killer follow-up

**"What happens when Justin Bieber tweets?"**

- He's a celebrity (≥10k followers, very much so).
- His tweet goes into `celeb:bieber` cache; nothing else happens at write time.
- The next time any follower opens their timeline, the read path includes recent celeb tweets.
- His tweet appears in timelines within seconds — but the *read path* did the work, not the write path.
- Without this design, his tweet would generate 100M Redis writes per second of his life.

**"What if a celebrity goes from 9,999 to 50,000 followers in an hour (viral moment)?"**

- The threshold check at fan-out time catches the transition.
- The user's *previously* pushed tweets stay in followers' timelines (they're already cached).
- New tweets are stored in the celeb cache instead.
- No backfill needed; users see consistent behavior going forward.

**"How does the 'For You' (ranked) timeline differ?"**

- Reuses this infrastructure as candidate generation.
- Adds candidates from outside the follow graph (recommended tweets).
- ML model scores all candidates.
- Re-ranking pass for diversity, ads, business rules.

## What "senior" looks like vs. "mid-level"

- **Mid-level** reproduces hybrid fan-out from Alex Xu's book.
- **Senior** explains the threshold as a knob, names two-tier or three-tier fan-out, uses shared per-celebrity caches, and treats the ranked feed as a layer rather than a parallel system.
