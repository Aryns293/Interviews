# Round 1 — Pure DSA
**Interview:** Cloud Infrastructure / Platform Engineering Company
**Duration:** 60 minutes

---

## Problem 1 — Number of Islands (Service Mesh Health Cluster)
**Difficulty:** Medium
**Time Budget:** 20 minutes

### The Problem
Count connected clusters of healthy nodes in a service mesh health-check grid.
```
grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
```

**Before you code:**
- DFS: mark visited by flipping `1 → 0` in-place. No extra memory.
- BFS: use a queue, same time complexity, slightly more memory.
- Which would you choose if the grid was being updated live (new nodes coming online)? (BFS is slightly easier to make incremental — but the real answer is Union-Find for dynamic connectivity.)

**Mid-solve twist:**
> "Now nodes can come online or go offline dynamically. After each toggle, you need the current cluster count. Don't re-run BFS from scratch each time."

*Expected:* Union-Find (Disjoint Set Union). Add/remove nodes incrementally. `find()` with path compression and `union()` by rank. O(α(n)) per operation, effectively O(1).

---

## Problem 2 — LRU Cache (Docker Image Eviction)
**Difficulty:** Medium
**Time Budget:** 35 minutes

### The Problem
Implement an LRU Cache. O(1) `get(key)` and `put(key, value)`.

**Platform framing:** Your Docker execution-image cache. When disk fills up, evict the least-recently-used execution image to make room for new ones.

**The data structure (must state before coding):**
- **HashMap:** key → node (for O(1) lookup)
- **Doubly linked list:** maintains access order (most recent at head, least recent at tail)
- On every `get` or `put`: move the accessed node to the head
- On eviction: remove from the tail

**What I'm grading:**
| Criteria | Weight |
|---|---|
| Correct O(1) approach (not O(n) timestamp approach) | High |
| Both prev and next pointers updated on every node move | High |
| Separate dummy head and tail nodes (simplifies edge cases) | Medium |
| Capacity = 1 edge case (put evicts immediately) | Medium |

---

## Mid-Solve Twist — Thread-Safe LRU

*After correct implementation:*

> "Make this LRU cache thread-safe. Multiple request handlers access it concurrently."

**What I'm looking for:**
- A single global `ReentrantLock` or `synchronized` block on `get` and `put` — simple, correct, but serializes all access
- **Better:** Read-write lock — multiple concurrent reads allowed, writes are exclusive (`ReentrantReadWriteLock` in Java)
- **Best for high-throughput:** Segmented/sharded LRU — split cache into N shards by `key % N`, each with its own lock. N concurrent writes possible.
- **Lock-free:** Java's `ConcurrentLinkedHashMap` — but explain that lock-free structures have higher code complexity and are rarely worth building from scratch

**What fails:**
- "I'd just add `synchronized`" with no discussion of granularity tradeoffs
- Not recognizing that the doubly linked list mutation (node moves) requires locking even for reads (because `get` modifies the list order)
