# Round 0 — Online Assessment
**Interview:** E-Commerce / High-Traffic Retail (e.g., Flipkart, Swiggy, Amazon retail side)
**Format:** 90 minutes. Focused heavily on arrays, strings, and DB query optimization.

---

## DSA Problems

### Problem 1 — Next Permutation
**Difficulty:** Medium

Implement the next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

**E-Commerce framing:** Often framed as "Given a product's SKU code, find the next sequential code in the catalog generator."

**What I'm testing:**
- Do you understand the O(N) in-place algorithm?
- Step 1: Find largest index `i` such that `nums[i] < nums[i+1]`.
- Step 2: Find largest index `j` such that `nums[j] > nums[i]`.
- Step 3: Swap `nums[i]` and `nums[j]`.
- Step 4: Reverse the sub-array `nums[i+1...end]`.

---

### Problem 2 — Word Break
**Difficulty:** Medium

Given a string `s` and a dictionary of strings `wordDict`, return true if `s` can be segmented into a space-separated sequence of dictionary words.

**E-Commerce framing:** Parsing search queries (e.g., "iphonecasesilicone" -> "iphone case silicone") against a product catalog dictionary.

**What I'm testing:**
- DP approach: `dp[i]` is true if `s[0...i]` can be segmented.
- Outer loop: `1 to s.length`, Inner loop: `0 to i`.
- Substring check: `if (dp[j] && wordDict.includes(s.substring(j, i))) dp[i] = true;`.

---

## MCQs

### Database Locking
**Q:** What is the difference between Optimistic and Pessimistic locking?
**A:** Optimistic assumes no conflict and checks a version column on `UPDATE`. Pessimistic locks the row (`SELECT FOR UPDATE`) preventing other reads/writes until the transaction commits. E-commerce uses Optimistic locking for cart updates (high contention is rare), but Pessimistic for actual payment/inventory deduction.

### Caching
**Q:** What is the "Cache Stampede" (Thundering Herd) problem?
**A:** When a highly requested cached item (like a Flash Sale product page) expires, thousands of concurrent requests miss the cache and hit the database simultaneously, crashing it. Fixed via locking, staggered TTLs, or background cache warming.
