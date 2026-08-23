# Round 1 — Pure DSA
**Interview:** HealthTech / Compliance-Heavy Company
**Duration:** 60 minutes

---

## Problem 1 — Minimum Window Substring (Log Parsing)
**Difficulty:** Medium (Reframed)

**The Problem:**
Minimum Window Substring — reframed as the smallest window of a patient event log containing all fields a compliance check requires.

---

## Problem 2 — Accounts Merge (Patient Deduplication)
**Difficulty:** Hard (Reframed)

**The Problem:**
Accounts Merge (Union-Find) — reframed as merging duplicate patient records sharing any identifying field (email, phone, ID) across a messy intake system, via Union-Find.

**Mid-solve twist:**
> "New records keep arriving after merging has started, and two already-merged groups can later need merging with each other."

*Forces genuine incremental Union-Find with path compression, not a one-shot batch merge.*

**Scored on:**
- Modeling this as a graph/Union-Find problem rather than naive pairwise comparison.
- Brute force stated first.
- Edge case of a record matching nothing.
