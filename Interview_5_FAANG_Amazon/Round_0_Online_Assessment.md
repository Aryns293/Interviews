# Round 0 — Online Assessment
**Interview:** FAANG-Style / Amazon Bar Raiser Loop
**Format:** Auto-graded — 90 minutes. This OA is timed MORE aggressively than the other 4. Expect problems where the naive solution gets TLE even if correct.

---

## DSA Problems

### Problem 1 — Longest Increasing Path in a Matrix
**Difficulty:** Hard

Given an `m x n` matrix of integers, find the length of the longest increasing path. You can move in 4 directions (up, down, left, right). You cannot move diagonally or wrap around.

**What I'm testing:**
- DFS with memoization — classic topological order on a DAG (each cell is a node, edges point to adjacent cells with greater values)
- Time: O(m*n), each cell visited once due to memoization
- Do you recognize that this is DFS + memo, NOT DP in the traditional sense? (There's no obvious recurrence order without memoization)

**Common mistakes:**
- Trying to define a DP bottom-up without recognizing the dependency order problem (you don't know which cells are "smaller" without a sort)
- Not handling the boundary conditions correctly

---

### Problem 2 — Word Search II
**Difficulty:** Hard

Given a 2D board of characters and a list of words, find all words that exist in the board. Words can be constructed from letters in adjacent cells (horizontally or vertically adjacent). The same cell cannot be used more than once per word.

**What I'm testing:**
- Build a Trie from the word list — use it to prune the DFS early
- Do you remove found words/leaves from the Trie as you find them (prevents duplicates and prunes dead branches)?
- Time: O(m * n * 4^L) where L is the max word length — Trie pruning drastically reduces this in practice

---

## MCQ Section

### Bitwise Operations — Output Prediction
```c
int x = 5;    // 0101
int y = 3;    // 0011
printf("%d %d %d %d", x & y, x | y, x ^ y, ~x);
// Expected: 1 7 6 -6
```

### Priority Scheduling Numerical
Given 4 processes with burst times and priorities, compute average waiting time under Priority scheduling (preemptive). Draw the Gantt chart. Time pressure: you have 4 minutes.

### Aggressive Timer
This OA cuts off mid-submission if the timer hits 0. There are no partial scores. Finish or lose the problem.
