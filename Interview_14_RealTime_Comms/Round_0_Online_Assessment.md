# Round 0 — Online Assessment
**Interview:** Real-Time Communications / Streaming (e.g., Zoom, Slack, Discord)
**Format:** 90 minutes. Focus on graphs, sliding windows, and bitwise logic.

---

## DSA Problems

### Problem 1 — Find K-th Smallest Pair Distance
**Difficulty:** Hard

The distance of a pair of integers a and b is defined as the absolute difference between a and b. Given an integer array nums and an integer k, return the kth smallest distance among all the pairs.

---

### Problem 2 — Minimum Window Subsequence
**Difficulty:** Hard

Given strings s1 and s2, return the minimum contiguous substring part of s1, so that s2 is a subsequence of the part.

---

## MCQs

### Networking
**Q:** Why is UDP preferred over TCP for real-time video/voice calls?
**A:** TCP guarantees delivery and order, which requires retransmitting dropped packets (Head-of-line blocking). In real-time video, if a frame from 2 seconds ago was dropped, we don't care anymore — we just want the *current* frame. UDP is fire-and-forget, ensuring lower latency.

### WebSockets
**Q:** What is the underlying protocol that WebSockets upgrade from?
**A:** HTTP/1.1. (A WebSocket connection starts as a standard HTTP GET request with an `Upgrade: websocket` header).
