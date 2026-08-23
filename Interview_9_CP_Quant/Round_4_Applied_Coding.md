# Round 4 — Applied Coding: Backend + Full-Stack
**Interview:** Competitive-Programming-Heavy / Quant-Adjacent Company
**Duration:** 60 minutes
**Style:** Algorithmically dense "applied" coding.

---

## Build 1 — Sliding Window Maximum (Monotonic Deque)
**Time Budget:** 20 minutes

> "Implement a sliding window maximum from scratch. Explain the amortized O(n) argument as you type."

**What I want to see:**
- You know to use a `deque` (double-ended queue).
- Store *indices* in the deque, not values.
- **The O(n) argument:** Every element is pushed into the deque exactly once and popped from the deque at most once. Therefore, the total number of operations across the entire array traversal is 2N, making the time complexity strictly O(N), or amortized O(1) per element.

---

## Build 2 — Generic LRU/LFU Rate Limiter
**Time Budget:** 30 minutes

> "Build a rate limiter using a sliding window log. But I want exact memory-complexity analysis, not just 'it works'."

```js
class RateLimiter {
  constructor(windowMs, maxRequests) { ... }
  // Returns true if allowed, false if rate limited
  allowRequest(userId, timestamp) { ... }
}
```

**What I'm testing:**
- Standard Sliding Window Log: store timestamps in an array/queue per user. Remove timestamps older than `now - windowMs`. If length < `maxRequests`, allow.
- **Memory Complexity analysis:** O(N * M) where N = number of active users, M = `maxRequests`. In quant/high-throughput, this is a memory leak waiting to happen if `maxRequests` is 10,000.
- **Optimization:** If pushed, can you pivot to the Sliding Window Counter (approximate, much lower memory O(N))?

---

## Discussion — SQL EXPLAIN Plan & Complexity

> "Given a hot Postgres query doing a full scan on 10M rows: `SELECT * FROM trades WHERE symbol = 'AAPL' AND timestamp > '2023-01-01' ORDER BY price DESC LIMIT 10;`. Walk through the EXPLAIN plan, identify the missing index, and state the actual complexity before and after the fix."

*Expected:*
- **Before:** O(N) full table scan + O(N log K) heap sort for the LIMIT.
- **The fix:** Create a composite index: `CREATE INDEX idx_trades ON trades(symbol, timestamp, price DESC);`
- **After:** O(log N) to find the start of the index range. If the index exactly matches the sort order, Postgres can just read the first 10 matching index entries. Query becomes effectively O(log N).
