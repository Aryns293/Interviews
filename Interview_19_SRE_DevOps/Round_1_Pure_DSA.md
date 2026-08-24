# Round 1 — Pure DSA
**Interview:** Site Reliability Engineering (SRE) / DevOps-Focused Company
**Duration:** 60 minutes

---

## Problem 1 — Network Delay Time
**Difficulty:** Medium

**The Problem:**
Network Delay Time (Dijkstra) — reframed as time for a status-change signal to propagate from one service to every dependent service in your dependency graph.

---

## Problem 2 — Sliding Window Maximum
**Difficulty:** Hard

**The Problem:**
Sliding Window Maximum (monotonic deque, O(n)) — reframed as a rolling max of request latency over the last N requests for real-time alerting, without recomputing from scratch.

**Mid-solve twist:**
> "Some requests arrive out of order (delayed telemetry) — the window isn't strictly append-only anymore."

*Forces rethinking the monotonic-deque invariant under out-of-order insertion.*

**Scored on:**
- Recognizing why the invariant breaks.
- Brute force stated first.
- Edge case of window larger than data seen so far.
