# Round 0 — Online Assessment
**Interview:** Real-Time Communications / Streaming (e.g., Zoom, Slack, Discord)
**Format:** 90 minutes. Focus on graphs, sliding windows, and bitwise logic.

---

## DSA Problems

### Problem 1 — Number of Connected Components in an Undirected Graph
**Difficulty:** Medium

Given `n` nodes and an array of `edges`, return the number of connected components.

**RTC framing:** "In a P2P video call network, given a list of direct connections between peers, how many isolated groups exist?"

**What I'm testing:**
- Do you use Union-Find (Disjoint Set) or DFS/BFS?
- Union-Find is highly preferred for dynamic connectivity (if edges were added over time).
- Path compression and union by rank.

---

### Problem 2 — Sliding Window Median
**Difficulty:** Hard

Given an integer array `nums` and an integer `k`, return the median of each window of size `k` moving from left to right.

**RTC framing:** "Smoothing out jitter in real-time network latency metrics."

**What I'm testing:**
- Two Heaps (Max-Heap for lower half, Min-Heap for upper half).
- Rebalancing the heaps as the window slides.
- Handling lazy deletion of elements moving out of the window (since deleting from a heap is O(N), you just mark them as deleted and ignore them when they reach the top).

---

## MCQs

### Networking
**Q:** Why is UDP preferred over TCP for real-time video/voice calls?
**A:** TCP guarantees delivery and order, which requires retransmitting dropped packets (Head-of-line blocking). In real-time video, if a frame from 2 seconds ago was dropped, we don't care anymore — we just want the *current* frame. UDP is fire-and-forget, ensuring lower latency.

### WebSockets
**Q:** What is the underlying protocol that WebSockets upgrade from?
**A:** HTTP/1.1. (A WebSocket connection starts as a standard HTTP GET request with an `Upgrade: websocket` header).
