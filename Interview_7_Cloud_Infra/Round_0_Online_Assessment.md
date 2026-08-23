# Round 0 — Online Assessment
**Interview:** Cloud Infrastructure / Platform Engineering Company
**Format:** Auto-graded — 90 minutes. MCQs lean heavily Linux/OS.

---

## DSA Problems

### Problem 1 — Number of Islands
**Difficulty:** Medium

Given a 2D grid of `'1'` (land) and `'0'` (water), count the number of islands. An island is surrounded by water and formed by connecting adjacent lands horizontally or vertically.

**Platform framing:** Count connected clusters of healthy nodes in a service mesh health-check grid. A `'1'` is a healthy node, a `'0'` is an unreachable one.

**What I'm testing:**
- DFS or BFS — either is fine. State your choice before coding.
- Mark visited cells in-place (flip `'1'` to `'0'`) or use a separate visited set — both are correct, but in-place is O(1) extra space.
- Edge cases: empty grid, all land, all water.

---

### Problem 2 — Design an LRU Cache
**Difficulty:** Medium (but often failed in implementation)

Implement an LRU Cache with O(1) `get(key)` and `put(key, value)`.

**Platform framing:** Your Docker execution-image cache — evict the least-recently-used image when disk fills up.

**What I'm testing:**
- Do you immediately reach for doubly linked list + hashmap? (Not just a hashmap with timestamps — that's O(n) for eviction)
- Is your doubly linked list implementation correct — do you update both `prev` and `next` pointers on every move?
- Edge cases: capacity = 1, updating an existing key (should move to front, not insert duplicate).

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
