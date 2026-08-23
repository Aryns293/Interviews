# Round 1 — Pure DSA
**Interview:** Fintech / Payments Company
**Duration:** 60 minutes
**Interviewer's Note:** Both problems are reframed in a payments context. The algorithm is the same — the framing tests whether you can apply your CS knowledge to a domain, not just recite solutions.

---

## Problem 1 — Subarray Sum Equals K (Fraud Pattern Detection)
**Difficulty:** Medium
**Time Budget:** 20 minutes

### The Problem
> You receive a daily transaction ledger as an integer array — positive values are credits, negative values are debits (refunds). Find the number of contiguous runs of transactions that sum to exactly a flagged amount `k`.

**Before you code — I'll ask:**
- "Why can't you use a sliding window here?" → Negative values. Sliding window assumes shrinking the window reduces the sum, which doesn't hold with negatives.
- "What does `prefix[j] - prefix[i] = k` mean geometrically in the ledger?" → The subarray between index `i+1` and `j` sums exactly to `k`.

**What I'm grading:**
| Criteria | Weight |
|---|---|
| Reached prefix-sum + hashmap approach, not sliding window | High |
| Initialized `prefixCount = {0: 1}` correctly (handles subarrays starting at index 0) | High |
| Handled negative numbers without special-casing | Medium |
| Edge case: k = 0 (subarrays that sum to zero — not inherently fraudulent, but a test case) | Medium |

---

## Problem 2 — Merge K Sorted Settlement Logs
**Difficulty:** Hard
**Time Budget:** 30 minutes

### The Problem
> You have `K` payment processors, each producing a sorted log of transactions in chronological order (sorted by timestamp). Merge them into a single chronological ledger.

This is Merge K Sorted Lists, reframed as a real settlement problem.

**Before you code — I'll ask:**
- "What's the brute force?" → Collect all transactions, sort. O(N log N) where N = total transactions.
- "What's the optimal?" → Min-heap of size K. Always extract the smallest timestamp, push the next entry from that processor's log. O(N log K).
- "Why log K and not log N?" → The heap never holds more than K elements — one per active processor.

---

## Mid-Solve Twist — Late Arrivals

*After you've implemented the standard merge:*

> "New requirement: settlement logs from overseas processors arrive late — sometimes 2–5 hours after the transaction timestamp. Your merge is already running when these late entries come in. How do you handle them?"

**What this tests:**
- Do you recognize this is a **bounded-lateness / watermarking** problem, not a data structure problem?
- The concept: define a watermark — "we will wait up to W hours for late entries before finalizing a time window." Any entry arriving after the watermark for its window is a late arrival, handled separately.
- This is directly how Apache Flink and Apache Beam handle streaming data — you don't need to know the exact API, but you should know the pattern.

**What I want to hear:**
1. "The clean merge only works if all data is available upfront."
2. "For streaming data with late arrivals, I'd define a lateness bound — e.g., 3 hours — and any transaction with `arrival_time > event_time + 3h` is routed to a late-arrival handler, not the main merge."
3. "In practice, this means our 'finalized' ledger has a T+3h delay before it's truly closed."

**What fails:**
- "I'd re-sort the whole ledger whenever a late entry arrives" — O(N log N) on every late arrival, completely impractical at scale
- Ignoring the problem and continuing with the static solution without flagging it
