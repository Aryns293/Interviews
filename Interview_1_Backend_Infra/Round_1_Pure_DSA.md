# Round 1 — Pure DSA
**Interview:** Backend / Infra-Heavy Product Company
**Duration:** 60 minutes
**Format:** Live coding on a shared editor (CoderPad or similar). No IDE, no autocomplete, no browser.
**Interviewer Note:** I already know your background. I know you've built a job queue (QueueFlow) and worked with DAGs (gitlight). My DSA picks are not random — they are deliberately chosen to map to concepts you claim to understand. Let's see if you actually do.

---

## Problem 1 — Course Schedule
**Difficulty:** Medium
**Time Budget:** 20 minutes

### The Problem
There are `numCourses` labeled `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [a, b]` means you must take course `b` before course `a`. Return `true` if it is possible to finish all courses, otherwise return `false`.

### What I want from you — before you type a single line
- Tell me what kind of graph structure this is (directed, undirected, weighted, DAG?)
- Is it guaranteed connected? What if it's not?
- Tell me your brute force approach before you go optimal. I want to hear you think.

### What I'm actually grading
| Criteria | Weight |
|---|---|
| Recognized it as cycle detection in a directed graph | High |
| Stated DFS-with-coloring OR Kahn's BFS approach before coding | High |
| Handled disconnected components (outer loop over all nodes) | Medium |
| Edge cases: single node, self-loop, empty prerequisites array | Medium |
| Clean code with correct visited/in-stack color tracking | Medium |

### Expected solution
DFS with 3-color marking: WHITE (unvisited) → GRAY (in current DFS stack) → BLACK (done). If you encounter a GRAY node, you found a cycle.

Alternatively: Kahn's Algorithm (BFS topological sort) — if not all nodes are processed, a cycle exists.

---

## Problem 2 — Alien Dictionary
**Difficulty:** Hard
**Time Budget:** 25 minutes

### The Problem
You are given a list of words from an alien language's dictionary. The words are sorted lexicographically by the rules of this alien language. Derive the character ordering of this alien language.

### What I want before you code
- What data structure maps characters to their ordering relationships? (Directed graph)
- How do you extract edges? (Compare adjacent words character by character)
- What's the base case where no valid ordering exists?

### What I'm actually grading
| Criteria | Weight |
|---|---|
| Built adjacency list correctly from adjacent word comparisons | High |
| Detected the "prefix conflict" edge case (word `"abc"` appearing before `"ab"`) | High |
| Used topological sort (Kahn's or DFS) correctly | High |
| Handled disconnected character sets | Medium |

---

## Mid-Solve Twist — The Differentiator Moment
*I will say this mid-way through Problem 2, without warning:*

> "Good. Now forget the static list — edges can be added dynamically, one at a time. After each edge insertion, I want you to answer: is the ordering still valid? No cycles yet?"

### What this tests
Can you recognize that re-running full topological sort on every insert is O(V+E) per operation, and that the smarter path is **incremental cycle detection using Union-Find** or an online DFS?

### What I'm looking for
- Don't panic. Say "let me think about this."
- Recognize the problem is now "detect a cycle after each edge addition"
- Propose Union-Find: if adding edge (u→v) and u and v are already in the same component, it's a cycle
- Note the limitation: Union-Find detects cycles in undirected graphs; directed cycle detection is harder incrementally — acknowledging this nuance is the differentiator

### Instant failure signals
- Re-running full topological sort on every insert without acknowledging the cost
- Saying "Union-Find handles it" without noting it's for undirected graphs
- Freezing and going silent for more than 60 seconds without communicating your thought process
