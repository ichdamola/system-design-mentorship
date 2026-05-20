# Week 13: Design Ride-Sharing (Uber)

## 🎯 The prompt

Design Uber's core matching service — riders request a ride, the system finds nearby available drivers, dispatches one, and tracks the ride to completion.

**Constraints the interviewer might give you:**

- 100M users, 5M active drivers
- 20M rides/day globally
- Driver location updates every 4 seconds while online
- Match latency target: < 5 seconds from request to driver-assigned
- ETA must update in real time during the ride

**Suggested time budget:** 45 minutes

| Phase | Time |
|---|---|
| Clarify scope | 5 min |
| Geospatial indexing + matching | 25 min |
| ETA + ride lifecycle deep dive | 15 min |

## ✅ Your job

1. **Try this yourself first.** 45-minute timer.
2. **Then open [answer.md](answer.md).**
3. **Read [interviewer-cues.md](interviewer-cues.md).**

## 💡 What you should already know

- Geospatial indexing: geohash, Google S2, quadtrees (briefly)
- Why a SQL `WHERE distance < X` index doesn't scale
- Pub/sub for driver location streams
- State machines (the ride lifecycle has many states)
- The matching problem (greedy nearest vs. optimization)
