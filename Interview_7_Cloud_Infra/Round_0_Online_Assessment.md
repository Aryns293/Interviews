# Round 0 — Online Assessment
**Interview:** Cloud Infrastructure / Platform Engineering Company
**Format:** Auto-graded — 90 minutes. MCQs lean heavily Linux/OS.

---

## DSA Problems

### Problem 1 — Minimum Number of Refueling Stops
**Difficulty:** Hard

A car travels from a starting position to a destination which is target miles east of the starting position. Return the minimum number of refueling stops the car must make in order to reach its destination.

---

### Problem 2 — Swim in Rising Water
**Difficulty:** Hard

You are given an n x n integer matrix grid where each value grid[i][j] represents the elevation at that point. You can swim from a square to another 4-directionally adjacent square if and only if the elevation of both squares individually are at most t. Return the least time until you can reach the bottom right square.

---

## MCQs

### Linux — top/htop Output Reading
```
PID   USER  PR  NI  VIRT  RES  SHR  S  %CPU  %MEM  TIME+    COMMAND
1234  root  20   0  512m  48m  12m  R  98.7   2.4   0:45.23  node
```
**Questions:**
- What does `S = R` mean? (Running — actively consuming CPU)
- What does `VIRT vs RES` mean? (Virtual memory allocated vs physical RAM actually used)
- This process is at 98.7% CPU. Is that a problem? When would it be expected? (Node.js is single-threaded — 98.7% CPU means the event loop is saturated. Expected for a CPU-bound task, a red flag for an I/O server.)

### Cron Syntax
```
*/15 9-17 * * 1-5
```
**What does this run?** → Every 15 minutes, between 9am–5pm, Monday through Friday.

### Process Scheduling — SRTF Numerical
Given 3 processes (P1: arrival=0, burst=8; P2: arrival=1, burst=4; P3: arrival=2, burst=2), draw the SRTF Gantt chart and compute average waiting time.
