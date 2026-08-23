# Round 1 — Pure DSA
**Interview:** General Product-Based Company
**Duration:** 60 minutes

---

## Problem 1 — Top K Frequent Elements
**Difficulty:** Medium
**Time Budget:** 15 minutes

### The Problem
Given an integer array `nums` and an integer `k`, return the `k` most frequent elements.

**What I want before you code:**
- State the naive approach first: sort by frequency → O(n log n)
- Then the heap approach: min-heap of size k → O(n log k)
- Then the optimal: bucket sort (index = frequency, max frequency = n) → O(n)

**I'll accept the heap solution. I'll be very impressed if you reach for bucket sort unprompted.**

**Edge cases to handle:**
- All elements have the same frequency — return any k of them
- k == len(nums) — return all
- Single element array

---

## Problem 2 — Median of Two Sorted Arrays
**Difficulty:** Hard
**Time Budget:** 35 minutes

### The Problem
Given two sorted arrays `nums1` and `nums2` of sizes `m` and `n`, find the median of the combined sorted array. Time complexity must be O(log(min(m,n))).

**The O(m+n) merge approach:**
- State this first. I expect you to. Then tell me why it's not good enough.
- "Merging takes O(m+n) time and O(m+n) space. The problem asks for O(log) — which implies binary search."

**The binary search approach:**
- Partition the smaller array into left and right halves
- Find the corresponding partition in the larger array such that `left_half_total == (m+n)/2`
- Use binary search on the smaller array to find the correct partition
- Median = average of `max(left1, left2)` and `min(right1, right2)` for even length, or `max(left1, left2)` for odd

**Edge cases:**
- Empty arrays
- One array is entirely smaller than the other (partition lands at the edge)
- Even vs odd total length

---

## Mid-Solve Twist

*After you solve the static version:*

> "Now the two arrays are actually two live streams that keep growing. You need to answer 'what's the current median?' after every new element added to either stream."

**What this forces:**
- Binary search on a static array no longer works — the array changes on every insert
- Correct approach: two-heap running median structure (max-heap for lower half, min-heap for upper half)
- This is the same data structure from Problem 1 of Interview 3, Round 0 — recognize the pattern
