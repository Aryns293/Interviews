# Round 0 — Online Assessment
**Interview:** Backend / Infra-Heavy Product Company
**Format:** Auto-graded platform (HackerRank / Codility) — 90 minutes, strict timer, no human contact until you clear this.

---

## DSA Problems

### Problem 1 — Course Schedule II
**Difficulty:** Medium

You are given `numCourses` and a list of `prerequisites`. Return a valid order to finish all courses. If impossible, return an empty array.

**What I'm testing:**
- Do you recognize this is a topological sort problem immediately, or do you fumble with BFS first?
- Do you handle the cycle-detection case *before* I ask you to?
- Do you write the adjacency list correctly, or do you reverse the edge direction?

**Gotcha:** A lot of candidates invert the edge direction when building the graph. I'm watching for that.

---

### Problem 2 — Kth Largest Element in a Stream
**Difficulty:** Medium

Design a class that finds the `k`th largest element in a stream. Calls to `add(val)` must return the current `k`th largest.

**What I'm testing:**
- Do you immediately reach for a min-heap of size `k`?
- Can you articulate *why* a min-heap of size `k` solves this without sorting the whole stream?
- Time complexity analysis: O(log k) per insertion — can you explain that clearly?

---

## MCQ Section (15–20 Questions)

### OS — CPU Scheduling Numericals
You will be given a Gantt chart scenario and asked to compute:
- Average waiting time (FCFS)
- Average turnaround time (SRTF)
- Context-switch overhead impact on throughput

**Example trap:** SRTF is preemptive SJF. If a new process arrives with burst time less than the remaining burst of the current process, it preempts immediately. Many candidates forget this.

### DBMS — Normal Form Identification
Given a relation schema with functional dependencies, identify whether it is in 1NF, 2NF, 3NF, or BCNF.

**Example trap:** A relation can be in 3NF but NOT in BCNF. Most candidates confuse these.

### C/Java — Pointer Arithmetic Output Prediction
```c
int arr[] = {10, 20, 30};
int *p = arr;
printf("%d", *(p + 2) - *(p + 1));
```
**Expected output:** `10`. If you got anything else, review pointer arithmetic before this OA.
