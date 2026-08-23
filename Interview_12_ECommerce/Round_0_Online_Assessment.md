# Round 0 — Online Assessment
**Interview:** E-Commerce / High-Traffic Retail (e.g., Flipkart, Swiggy, Amazon retail side)
**Format:** 90 minutes. Focused heavily on arrays, strings, and DB query optimization.

---

## DSA Problems

### Problem 1 — Sliding Window Maximum (Monotonic Queue)
**Difficulty:** Hard

You are given an array of integers nums, there is a sliding window of size k which is moving from the very left of the array to the very right. Return the max sliding window.

---

### Problem 2 — Russian Doll Envelopes (LIS)
**Difficulty:** Hard

You are given a 2D array of integers envelopes where envelopes[i] = [wi, hi] represents the width and the height of an envelope. Return the maximum number of envelopes you can Russian doll (i.e., put one inside other).

---

## MCQs

### Database Locking
**Q:** What is the difference between Optimistic and Pessimistic locking?
**A:** Optimistic assumes no conflict and checks a version column on `UPDATE`. Pessimistic locks the row (`SELECT FOR UPDATE`) preventing other reads/writes until the transaction commits. E-commerce uses Optimistic locking for cart updates (high contention is rare), but Pessimistic for actual payment/inventory deduction.

### Caching
**Q:** What is the "Cache Stampede" (Thundering Herd) problem?
**A:** When a highly requested cached item (like a Flash Sale product page) expires, thousands of concurrent requests miss the cache and hit the database simultaneously, crashing it. Fixed via locking, staggered TTLs, or background cache warming.
