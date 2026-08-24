# Round 0 — Online Assessment
**Interview:** Early-Stage Seed Startup
**Format:** Auto-graded — 75 minutes (slightly shorter window than enterprise loops). Speed matters here. The MCQs are lighter but include a JS type-coercion trap that weeds out people who've only worked in typed languages.

---

## DSA Problems

### Problem 1 — Minimum Window Substring
**Difficulty:** Hard

Given two strings s and t of lengths m and n respectively, return the minimum window substring of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string.

---

### Problem 2 — N-Queens
**Difficulty:** Hard

The n-queens puzzle is the problem of placing n queens on an n x n chessboard such that no two queens attack each other. Given an integer n, return all distinct solutions to the n-queens puzzle.

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
