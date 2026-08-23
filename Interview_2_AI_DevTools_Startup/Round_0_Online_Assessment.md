# Round 0 — Online Assessment
**Interview:** AI / Dev-Tools Startup
**Format:** Auto-graded (HackerRank or similar) — 90 minutes, no human contact until you clear this.

---

## DSA Problems

### Problem 1 — Word Search II (Trie + DFS)
**Difficulty:** Hard

Given an m x n board of characters and a list of strings words, return all words on the board. Each word must be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

---

### Problem 2 — Maximum Profit in Job Scheduling (DP + Binary Search)
**Difficulty:** Hard

We have n jobs, where every job is scheduled to be done from startTime[i] to endTime[i], obtaining a profit of profit[i]. You're given the startTime, endTime and profit arrays, return the maximum profit you can take such that there are no two jobs in the subset with overlapping time range.

---

## MCQ Section

### Computer Networks — HTTP Status Codes
- 401 vs 403: What's the semantic difference?
  - 401 = **Unauthorized** — authentication required or failed
  - 403 = **Forbidden** — authenticated but not permitted
- 301 vs 302: What's the caching difference?
  - 301 = **Permanent redirect** — browsers cache this forever
  - 302 = **Temporary redirect** — browsers re-check every time

### JavaScript — Event Loop Output Prediction
```js
console.log('1');
setTimeout(() => console.log('2'), 0);
Promise.resolve().then(() => console.log('3'));
console.log('4');
```
**Expected output:** `1, 4, 3, 2`
- Microtask queue (Promise) drains before the macrotask queue (setTimeout)

### CSS — Specificity Trap
```css
#id .class { color: red; }
.class.class { color: blue; }
```
Which wins? `#id .class` — an ID selector (0,1,0,0) always beats class selectors (0,0,2,0) regardless of how many classes you chain.
