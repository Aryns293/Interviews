# Round 0 — Online Assessment
**Interview:** Early-Stage Seed Startup
**Format:** Auto-graded — 75 minutes (slightly shorter window than enterprise loops). Speed matters here. The MCQs are lighter but include a JS type-coercion trap that weeds out people who've only worked in typed languages.

---

## DSA Problems

### Problem 1 — Meeting Rooms II
**Difficulty:** Medium

Given an array of meeting intervals `[start, end]`, find the minimum number of conference rooms required.

**Startup framing:** How many concurrent build/deploy pipeline slots does a 3-person team actually need?

**What I'm testing:**
- Min-heap approach: sort by start time, use a min-heap of end times, size of heap = rooms in use at any point
- Time: O(n log n), Space: O(n)
- Edge cases: no meetings, all meetings at the same time, back-to-back meetings (does `[9,10]` and `[10,11]` need 1 or 2 rooms? They're adjacent, not overlapping — 1 room.)

---

### Problem 2 — Partition Equal Subset Sum
**Difficulty:** Medium-Hard

Given an integer array `nums`, return `true` if the array can be partitioned into two subsets with equal sum.

**What I'm testing:**
- Do you recognize this as a 0/1 knapsack variant? (Target = total_sum / 2, find if a subset sums to target)
- DP table: `dp[i][j]` = can we achieve sum `j` using first `i` elements?
- Optimization: 1D DP (rolling array), O(n * sum) time, O(sum) space
- Early exit: if `total_sum` is odd → return false immediately

---

## MCQs

### JavaScript — Type Coercion Output Prediction
```js
console.log([] + []);        // ""  (both arrays to "", concatenated)
console.log([] + {});        // "[object Object]" ([] → "", {} → "[object Object]")
console.log({} + []);        // 0  (in expression context, {} is a block → +[] = 0)
console.log(+"");            // 0  (unary + on empty string → 0)
console.log(null == undefined); // true (loose equality, both "nullish")
console.log(null === undefined); // false (strict equality, different types)
```
**Trap:** Line 3 depends entirely on context — if `{} + []` is a statement, `{}` is parsed as a block, not an object literal.

### REST Status Codes (Quick)
- 200 vs 201 vs 204
- 400 vs 409 vs 422
- When is 307 correct vs 302?

### SQL — Quick JOIN
Given: `users(id, name)` and `orders(id, user_id, amount)`, write a query returning all users and their total order amount, including users with no orders.
→ `LEFT JOIN` + `COALESCE(SUM(o.amount), 0)`
