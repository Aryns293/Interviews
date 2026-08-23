# Round 0 — Online Assessment
**Interview:** Systems / Low-Level Deep-Dive Company
**Format:** Auto-graded — 90 minutes.

---

## DSA Problems

### Problem 1 — Serialize and Deserialize a Binary Tree
**Difficulty:** Hard

Design an algorithm to serialize a binary tree to a string, and deserialize that string back to the original tree structure. Your serialization format is your choice — just make it reversible.

**What I'm testing:**
- Do you recognize the direct parallel to how Git encodes tree objects into a binary/text format?
- Do you use pre-order traversal with null markers, or level-order (BFS)?
- Is your deserializer stateful (uses an index or queue), or do you re-scan the string on every call?

**Common mistakes:**
- Forgetting that delimiter choice matters — what if node values contain your delimiter character?
- Off-by-one errors in the deserializer when consuming tokens

---

### Problem 2 — Find Median from Data Stream
**Difficulty:** Hard

Design a data structure that supports: `addNum(int num)` — add a number; `findMedian()` — return the current median.

**What I'm testing:**
- Do you immediately recognize the two-heap approach (max-heap for lower half, min-heap for upper half)?
- Can you articulate the invariant: `|maxHeap.size - minHeap.size| <= 1`?
- Do you handle even vs odd total count correctly?
- Time: O(log n) per add, O(1) per median

---

## MCQs

### C/C++ — Static Initialization Order
```cpp
// file_a.cpp
int x = 10;

// file_b.cpp
extern int x;
int y = x * 2; // Is y guaranteed to be 20?
```
**Answer:** No. The static initialization order across translation units is undefined in C++. This is the "Static Initialization Order Fiasco." `y` might be initialized before `x`.

### Linux — Command Output Prediction
- `find /var/log -name "*.log" -mtime -1` → find `.log` files modified in the last 24 hours
- `lsof -i :3000` → list processes using port 3000
- `grep -r "BRPOP" ./src --include="*.js"` → recursively grep for BRPOP in JS files

### CPU Scheduling — SRTF Numerical
Given 4 processes with arrival times and burst times, compute average waiting time under SRTF (Shortest Remaining Time First — preemptive SJF). Draw the Gantt chart.
