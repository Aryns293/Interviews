# Round 0 — Online Assessment
**Interview:** Backend / Infra-Heavy Product Company
**Format:** Auto-graded platform (HackerRank / Codility) — 90 minutes, strict timer, no human contact until you clear this.

---

## DSA Problems

### Problem 1 — Critical Connections in a Network (Graph)
**Difficulty:** Hard

There are n servers numbered from 0 to n - 1 connected by undirected server-to-server connections forming a network where connections[i] = [ai, bi] represents a connection between servers ai and bi. Any server can reach other servers directly or indirectly through the network. A critical connection is a connection that, if removed, will make some servers unable to reach some other server. Return all critical connections in the network in any order.

---

### Problem 2 — Trapping Rain Water (Two Pointers)
**Difficulty:** Hard

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.

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
