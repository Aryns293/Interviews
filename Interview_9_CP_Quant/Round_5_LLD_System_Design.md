# Round 5 — LLD & Light System Design
**Interview:** Competitive-Programming-Heavy / Quant-Adjacent Company

---

## LLD Problem — In-Memory Order Book

> "Design an in-memory Order Book (matching engine). Buy orders, Sell orders, price-time priority matching. I care about the data structure choices."

**Core Requirement:** Price-time priority. 
- You must match a Buy against the lowest Sell.
- You must match a Sell against the highest Buy.
- If prices tie, match the oldest order first.

**Data Structure Choice:**
- Why not just an Array/List? O(N) to insert, O(N) to find the best price. Too slow.
- **Two Heaps?** Max-heap for Bids, Min-heap for Asks. O(log N) insert, O(1) peek best. *But deleting a canceled order from a heap is O(N).*
- **Balanced BST (e.g., TreeMap/std::map):** `std::map<double, Queue<Order>>`. O(log N) to insert a new price level, O(1) to append to the Queue (time priority). Canceling is O(1) if you have a direct pointer/hashmap to the Order node. This is the industry standard for L2 order books.

---

## Design Pattern Follow-Ups

**Strategy — Matching Algorithms:**
> "Swap matching algorithms — price-time priority vs pro-rata — without touching the order-book core."

*Expected:* Strategy pattern. 
`OrderBook` delegates to an `IMatchingStrategy`. 
`PriceTimeStrategy` and `ProRataStrategy` implement the matching logic.

**Singleton — Order Book Instance:**
> "Why one Singleton instance per trading symbol (e.g., AAPL), and not one giant shared instance for all symbols?"

*Expected:* Concurrency. If you have one giant order book, a trade on AAPL blocks a trade on TSLA. One instance per symbol allows parallel matching across different symbols.

---

## Thread-Safety — High-Frequency Contention (Hard)

**Q:**
> "Two orders for AAPL arrive within the same microsecond at the same price level. How do you guarantee deterministic, fair ordering — without a single global lock becoming your throughput bottleneck?"

*Expected:*
- Fine-grained locking: lock only the specific price level queue, not the whole book.
- Even better: A Single-Threaded Event Loop pattern (like Redis or Node.js). High-frequency trading (HFT) engines often pin one thread to one CPU core per symbol, running a busy-wait loop over a lock-free ring buffer (LMAX Disruptor pattern). 
- Avoid OS-level mutexes because context switching is too slow for HFT.

---

## Light System Design — Real-Time Leaderboard

> "Design a real-time Leaderboard system that computes ranks for millions of users with fast updates and fast rank queries. Think about how LeetCode ranks you."

**Data Structure / Tech Choice:**
- A standard relational database `ORDER BY score LIMIT 10` is too slow for millions of rows in real-time.
- **Redis ZSET (Sorted Set):** You used this in QueueFlow. It's built on a Skip List and a Hash Table.
- `ZADD leaderboard 1500 user_id` → O(log N).
- `ZREVRANK leaderboard user_id` (get rank) → O(log N).
- `ZREVRANGE leaderboard 0 9` (get top 10) → O(log N + 10).

**Why Skip List over BST?**
- Redis uses Skip Lists for ZSETs because they are easier to implement lock-free (or with fine-grained locks) than a Red-Black Tree, and rebalancing a BST under heavy write load is expensive.
