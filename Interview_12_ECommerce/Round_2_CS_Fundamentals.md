# Round 2 — CS Fundamentals
**Interview:** E-Commerce / High-Traffic Retail
**Duration:** 45 minutes
**Theme:** High concurrency, inventory consistency, caching strategies.

---

## DBMS — Flash Sale Inventory

**Q1:** "We are running a flash sale for the iPhone 15. We have 100 units. 100,000 users click 'Buy' at the exact same millisecond. Walk me through exactly how you prevent selling 101 phones."

*Expected:*
- **Bad answer:** Read balance, if > 0, decrement. (Race condition).
- **Okay answer:** Pessimistic locking. `SELECT * FROM inventory WHERE item_id = X FOR UPDATE`. Decrement. Commit. (Correct, but 100,000 users waiting on a single row lock will bring the DB to a crawl).
- **Good answer (SQL only):** Optimistic `UPDATE inventory SET stock = stock - 1 WHERE item_id = X AND stock > 0;`. If rows affected == 0, it's sold out. No explicit read lock needed, relies on atomic row updates.
- **Best answer (E-commerce standard):** Use Redis. Load 100 into a Redis key. Use `DECR` (which is atomic). If the result is >= 0, place the order in a message queue (QueueFlow!) for async Postgres insertion. If result < 0, reject immediately.

---

## Computer Networks / CDNs

**Q:** "When a user hits the homepage, the product images load very fast, but the price sometimes takes a second to pop in. Architecturally, why is this happening?"

*Expected:*
- Images are static assets served from a CDN edge node (close to the user).
- Prices are dynamic (user-specific discounts, flash sales, out-of-stock). They require an API call to the origin server or a centralized cache, which adds network latency.

---

## Caching Strategies

**Q:** "Cache-aside vs Write-through. If I update a product's description in the admin panel, how does the frontend cache get updated in both strategies?"

*Expected:*
- **Cache-aside (Lazy):** Admin updates DB. Admin service deletes the Redis key. Next user reads from Redis (misses), reads from DB, writes to Redis.
- **Write-through (Proactive):** Admin updates DB AND updates the Redis key directly in the same transaction/flow.
- E-commerce usually prefers Cache-aside for product catalogs because writing to cache synchronously on every admin update can be fragile, and you don't want the admin update to fail just because Redis had a blip.
