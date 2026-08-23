# Round 0 — Online Assessment
**Interview:** Systems / Low-Level Deep-Dive Company
**Format:** Auto-graded — 90 minutes.

---

## DSA Problems

### Problem 1 — LFU Cache (Design)
**Difficulty:** Hard

Design and implement a data structure for a Least Frequently Used (LFU) cache. Implement the LFUCache class with get and put methods. The cache must operate in O(1) average time complexity for each operation.

---

### Problem 2 — Find Median from Data Stream (Heaps)
**Difficulty:** Hard

The median is the middle value in an ordered integer list. Implement the MedianFinder class that can add a number into the data structure and return the median of all elements so far in O(1) or O(log n) time.

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
