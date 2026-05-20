# Week 14: Interviewer cues — Video Streaming

## What they're listening for

| Signal | What it means |
|---|---|
| You pick HLS / DASH with chunks early | "Knows ABR is the answer." |
| You parallelize transcoding by chunk | "Has thought about the upload→watch latency for creators." |
| You bring up tiered storage (CDN / S3 / Glacier) | "Sees the cost dimension." |
| You direct-upload to S3 via presigned URLs | "Knows the bandwidth math." |
| You separate upload from playback as different pipelines | "Doesn't conflate the workloads." |
| You name the 95/5 rule and tier accordingly | "Knows the distribution that makes this affordable." |
| You flag viral cold-start as a CDN problem | "Has done CDN debugging." |

## Yellow flags

- Single file per video (no ABR).
- Synchronous transcoding in the upload API.
- Single storage tier.
- "We'll cache it in Redis" for video chunks (chunks are MB-sized, wrong tool).
- Designing the recommendation system instead of playback.

## Red flags

- Uploads going through your service before reaching S3 (would need exabit-scale ingress).
- One transcoding worker per video.
- No CDN in the design.
- Hot, warm, and cold content in the same storage class.
- Synchronous DRM key fetch on every chunk.

## The killer follow-up

**"What about live streaming?"**

- Same chunked-storage idea, but:
  - Ingest is real-time from a camera (RTMP, SRT, or WebRTC).
  - Chunks are 2-6s; "manifest" updates live.
  - Glass-to-glass latency target: 5-15s for standard HLS; under 3s for LL-HLS; sub-second for WebRTC.
  - CDN supports rolling manifest updates.
- The hardest part is balancing latency vs. ABR quality vs. CDN distribution.

**"What about DRM?"**

- Chunks encrypted with a per-content (or per-user) key.
- License server hands out keys to authenticated players.
- Widevine, FairPlay, PlayReady — three major DRMs; usually all three encodings exist.
- Adds latency to first-frame (license fetch) and CPU cost to decode.

**"How do you decide what to put on CDN PoPs?"**

- Per-region access patterns. A K-pop video is hot in Asia, cold in South America.
- ML-based pre-positioning for upcoming hot content (new release from a popular creator).
- LRU eviction at each PoP.
- Periodic recalibration based on per-PoP access logs.

## What "senior" looks like vs. "mid-level"

- **Mid-level** picks HLS and a CDN.
- **Senior** parallelizes transcoding by chunk, tiers storage by access pattern, single-flights origin fetches under viral load, and treats live streaming and DRM as additive complexity rather than pretending to design them in 45 minutes.
