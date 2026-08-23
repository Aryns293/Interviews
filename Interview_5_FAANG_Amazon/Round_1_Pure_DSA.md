# Round 1 — Pure DSA
**Interview:** FAANG-Style / Amazon Bar Raiser Loop
**Duration:** 60 minutes
**Interviewer's Note:** This round is harder than the other 4. I expect brute force in under 60 seconds, and I will introduce the constraint change without warning, mid-line. If you flinch or go silent for more than 45 seconds, that's a flag.

---

## Problem 1 — Longest Increasing Path in a Matrix
**Difficulty:** Medium-Hard
**Time Budget:** 20 minutes

### The Problem
Given an `m x n` integer matrix, return the length of the longest increasing path. Move in 4 directions (no diagonals, no wrap).

**Before you type a single line:**
1. "What's the brute-force?" → DFS from every cell, O(m*n*4^(m*n)) — state it, then move on.
2. "What optimization?" → Memoization. Once you compute the longest path from a cell, you never recompute it.
3. "Is this DP?" → It's DFS + memo. The key insight is that it's a DAG (strictly increasing → no cycles → topological structure). You could also solve it with topological sort, but DFS + memo is cleaner.

**What I'm grading harder than usual:**
- Brute force stated in under 60 seconds — non-negotiable
- Memoization correctly structured (the cache key is the cell, not the path)
- Boundary condition handling (don't go out of bounds, don't revisit in the same DFS path)

---

## Problem 2 — Word Search II
**Difficulty:** Hard
**Time Budget:** 35 minutes

### The Problem
Given a 2D board and a list of words, return all words that exist in the board (can move in 4 directions, each cell used at most once per word).

**The naive approach (state this first):**
- For each word, run a DFS/backtrack across the board → O(words * m * n * 4^L)
- With 10,000 words, this TLEs.

**The Trie approach (get here within 5 minutes of the naive):**
- Build a Trie from all words
- Run ONE DFS across the board, following Trie edges
- When a Trie node marks end-of-word, add it to results
- Remove found words from the Trie (prune dead branches, prevent duplicates)

---

## Mid-Solve Twist — Delivered Without Warning

*Mid-way through Problem 2, I will say:*

> "The word list is now 100,000 words instead of 10. Your current approach just timed out in our test environment. Optimize further."

**What I want:**
1. "What's my current bottleneck?" → At 100,000 words, the Trie is large but lookup is still O(L). The actual bottleneck is board DFS starting from every cell.
2. "What can I prune?" → After finding a word, I'm already removing it from the Trie. Additionally: if a Trie node has no children (leaf, non-end), I can prune the entire subtree.
3. The key optimization: aggressive Trie pruning — after backtracking, if a node has no remaining children AND is not an end-of-word marker, remove it. This dramatically reduces the Trie size as words are found.

**Failure signals:**
- Re-reading all 100,000 words into a hash set — abandons the Trie advantage
- "I'd increase the time limit" — not an answer
- Silence longer than 45 seconds without communicating your thought process
