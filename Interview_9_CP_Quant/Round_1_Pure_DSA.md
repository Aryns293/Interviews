# Round 1 — Pure DSA
**Interview:** Competitive-Programming-Heavy / Quant-Adjacent Company
**Duration:** 60 minutes
**Interviewer's Note:** I know you are a Codeforces Specialist and LeetCode Knight. I'm not going to give you standard Blind 75 questions. I'm testing whether you deeply understand advanced data structures, not just whether you've memorized their templates.

---

## Problem 1 — Maximum XOR (Bitwise Trie)
**Difficulty:** Medium-Hard
**Time Budget:** 20 minutes

### The Problem
Implement a Trie to find the maximum XOR of two numbers in an array.

**What I want to see:**
- Fast, clean implementation of the Trie node (`children = new TrieNode[2]`).
- The greedy choice logic: to maximize XOR, always try to go down the path of the *opposite* bit. If it exists, take it and add `1 << i` to the sum. If not, take the same bit and add 0.

---

## Problem 2 — Fenwick Tree (Binary Indexed Tree)
**Difficulty:** Hard
**Time Budget:** 40 minutes

### The Problem
Implement a Fenwick Tree from scratch supporting point updates and range sum queries.

**What I want to see:**
- Flawless, fast implementation of `update` and `query` using the `idx & (-idx)` trick.
- `update(idx, val)`: `for (; idx <= n; idx += idx & -idx) tree[idx] += val;`
- `query(idx)`: `for (; idx > 0; idx -= idx & -idx) sum += tree[idx];`

---

## Mid-Solve Twist — Range Updates on Fenwick Tree

*This is the real test for a 1400+ rated CF coder.*

> "Standard Fenwick does point-update / range-query. Standard difference arrays do range-update / point-query. I want **range-update AND range-query** in O(log N). How do you do it with a Fenwick tree?"

**The Insight I'm looking for:**
To add `v` to `[L, R]`, the impact on a prefix sum `query(x)` depends on `x`:
1. If `x < L`: impact is 0
2. If `L <= x <= R`: impact is `v * (x - L + 1)`
3. If `x > R`: impact is `v * (R - L + 1)`

Notice the `v * x` term. We can't do this with one Fenwick tree.
**The Solution:** Use *two* Fenwick trees (BIT1 and BIT2).
- `BIT1` maintains the difference array for point queries: `BIT1.update(L, v)`, `BIT1.update(R + 1, -v)`.
- `BIT2` maintains `v * (x - 1)`: `BIT2.update(L, v * (L - 1))`, `BIT2.update(R + 1, -v * R)`.
- Final query logic for prefix sum up to `x`: `query(x) = BIT1.query(x) * x - BIT2.query(x)`.

**What I'm grading:**
- Do you immediately pivot to a Segment Tree with Lazy Propagation? (Acceptable, but point out that it uses 4x memory and has larger constant factors).
- Can you logically derive the two-BIT math on a whiteboard, even if you haven't memorized the template? This tests your mathematical maturity, heavily valued in quant.
