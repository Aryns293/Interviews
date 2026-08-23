# Round 1 — Pure DSA
**Interview:** Early-Stage Seed Startup
**Duration:** 60 minutes
**Interviewer's Note:** I'm not testing whether you know algorithms — every senior dev knows algorithms. I'm testing how you reason about tradeoffs under pressure, and whether you can make a pragmatic decision when there isn't time to be perfect.

---

## Problem 1 — Meeting Rooms II
**Difficulty:** Medium
**Time Budget:** 15 minutes

### The Problem
Given meeting intervals, return the minimum number of conference rooms required.

**Before you code:**
- State the brute force first (even if it's O(n²)) — I expect this in 30 seconds
- Then describe the heap approach before you type anything
- "I'll sort by start time, maintain a min-heap of end times, and the heap size at any point is the number of rooms in use"

**The key insight I'm listening for:**
> "Why a min-heap of end times and not a counter?" — Because I need to know when the earliest-ending meeting finishes so I can reuse that room for the next meeting. The minimum of the heap tells me that.

---

## Problem 2 — Partition Equal Subset Sum
**Difficulty:** Medium-Hard
**Time Budget:** 30 minutes

### The Problem
Given an integer array, return `true` if it can be partitioned into two subsets with equal sum.

**Before you code:**
1. Early exit: if `sum % 2 !== 0`, return false immediately
2. Target = `sum / 2`
3. This is exactly 0/1 knapsack: "can we pick a subset that sums to exactly `target`?"

---

## Mid-Solve Twist — The Startup Pragmatism Test

*After you've explained the DP approach but before you finish implementation:*

> "This needs to work in a live demo in 20 minutes. The DP solution is O(n * sum). For the given input size, sum could be up to 20,000. Do you ship the correct DP, or a faster heuristic that might be wrong on some inputs?"

**This is the most important moment of this round.** There is a right answer to HOW you reason, not just what you pick.

**Strong answer:**
> "I ship the correct DP. Here's why: a heuristic that's sometimes wrong is worse than a correct solution that's slightly slower — especially in a live demo where a wrong answer is immediately visible. O(n * sum) with n ≤ 200 and sum ≤ 20,000 is ~4 million operations, which runs in well under a second in any modern language. The DP IS fast enough — there's no tradeoff to make here. If sum were 10^9, I'd have a different conversation."

**Acceptable answer:**
> "I'd ship the DP, but explicitly state out loud: 'This is the correct approach. If we had tighter time pressure and a very large sum, I'd discuss approximation — but for these input bounds, the DP is fine.'"

**Weak answer:**
- "I'd ship a greedy heuristic because it's faster" — without knowing if the heuristic is correct for all cases
- Spending 5 minutes debating the tradeoff when the DP is already the right call

**What this tests:** Can you quickly identify when there IS a real tradeoff vs when there isn't one — and communicate that judgment clearly?
