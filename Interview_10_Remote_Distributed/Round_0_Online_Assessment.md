# Round 0 — Online Assessment
**Interview:** Remote-First / Fully Distributed Team Company
**Format:** Async-friendly format. You might get 48 hours to complete a 2-hour window. The pressure is less about someone staring at you over Zoom, and more about how cleanly you communicate your code.

---

## DSA Problems

### Problem 1 — Rotting Oranges (Multi-Source BFS)
**Difficulty:** Medium

Given an `m x n` grid where `0` is empty, `1` is fresh, and `2` is rotten. Every minute, any fresh orange adjacent to a rotten one becomes rotten. Return the minimum minutes until no cell has a fresh orange (or -1 if impossible).

**Distributed Team Framing:** Simulate a status update propagating across a distributed team's task board, minute by minute, with no central sync point.

**What I'm testing:**
- Do you use Multi-Source BFS? (Initialize queue with ALL rotten oranges at time 0).
- If you use single-source BFS from each rotten orange, you will fail the time complexity check.
- Time: O(m * n), Space: O(m * n).

---

### Problem 2 — Edit Distance
**Difficulty:** Hard

Given two strings `word1` and `word2`, return the minimum number of operations (insert, delete, replace) required to convert `word1` to `word2`.

**Distributed Team Framing:** Compute the minimal diff between two versions of an async design doc two teammates edited independently. (This is exactly what `gitlight` does under the hood).

**What I'm testing:**
- Standard 2D DP array: `dp[i][j]` is the edit distance between `word1[0..i]` and `word2[0..j]`.
- Space optimization: you only need the previous row, so it can be optimized to O(min(m, n)) space.

---

## MCQs

### Git — Async Collaboration
Q: You have a local commit. The remote branch has advanced with 5 new commits by a teammate in another timezone. You want a linear history. Which command?
A: `git pull --rebase`

### HTTP Status Codes — Async Processing
Q: You submit a webhook payload. The server accepts it and puts it in a queue to process later. What is the correct HTTP status code?
A: `202 Accepted` (The request has been accepted for processing, but the processing has not been completed).
