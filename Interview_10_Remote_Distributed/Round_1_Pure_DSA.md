# Round 1 — Pure DSA
**Interview:** Remote-First / Fully Distributed Team Company
**Duration:** 60 minutes
**Interviewer's Note:** I care deeply about how you communicate your thoughts *before* you type. In a remote team, writing design docs is paramount. Consider this coding round a real-time design doc.

---

## Problem 1 — Rotting Oranges (Task Board Propagation)
**Difficulty:** Medium
**Time Budget:** 20 minutes

### The Problem
Simulate a status update propagating across a grid. Return minutes until complete.

**What I want to hear:**
"Since we need the shortest path/time for multiple sources simultaneously, this is a multi-source Breadth-First Search (BFS). I will find all initially rotten oranges and push them into a queue. Then I'll process them level by level."

---

## Problem 2 — Edit Distance (Async Doc Diff)
**Difficulty:** Hard
**Time Budget:** 40 minutes

### The Problem
Find the minimum edit distance to convert `word1` to `word2`.

**The Mid-Solve Twist:**
> "The two documents are actually being edited concurrently by two people in different timezones while you're computing this diff."

**What this tests:**
- Does the candidate recognize that this completely breaks the clean 'two static strings' DP assumption?
- *Expected insight:* "If the strings are mutating while I run the DP, the DP table will be invalid. In a real distributed system, we can't lock the document for the duration of the diff (O(N*M) time). We need to rely on Operational Transformation (OT) or CRDTs (Conflict-free Replicated Data Types) to resolve concurrent edits, or we snapshot the documents at a specific vector clock/timestamp before diffing."
- You don't have to code a CRDT here, but you MUST flag that the algorithmic assumption is violated by the distributed reality.
