# Round 1 — Pure DSA
**Interview:** Real-Time Communications / Streaming
**Duration:** 60 minutes
**Interviewer's Note:** I want to see you handle real-time streaming constraints (data arriving sequentially with no ability to look ahead).

---

## Problem 1 — Finding the 'K' most frequent elements in a Stream
**Difficulty:** Medium-Hard
**Time Budget:** 25 minutes

### The Problem
You are given a stream of chat messages. You need to keep track of the top K most frequently used words *at all times*.

**What I want to see:**
- Brute force (Hash map + sort) is O(N log N) on every query. Unacceptable.
- **Better:** Hash map (word -> count) + Min-Heap of size K.
- When a word arrives, update map. If word is in heap, update heap (requires custom heap or O(K) search). If not, push to heap. If heap > K, pop.
- **Best (LFU Cache approach):** Hash map (word -> count) + Hash map (count -> Doubly Linked List of words). O(1) update time.

---

## Problem 2 — Network Delay Time
**Difficulty:** Medium
**Time Budget:** 35 minutes

### The Problem
You are given a network of `n` nodes, labeled from `1` to `n`. You are also given `times`, a list of travel times as directed edges `times[i] = (ui, vi, wi)`, where `ui` is the source node, `vi` is the target node, and `wi` is the time it takes for a signal to travel from source to target. We will send a signal from a given node `k`. Return the minimum time it takes for all the `n` nodes to receive the signal.

**RTC framing:** "How long before all peers in the mesh network receive a broadcast message?"

**What I want to see:**
- This is a classic Dijkstra's algorithm problem (Shortest Path).
- You must use a Priority Queue (Min-Heap).
- Initialize distances to infinity, start node to 0.
- Pop node with smallest distance, update neighbors, push back into PQ.
- Time complexity: O(E log V).
