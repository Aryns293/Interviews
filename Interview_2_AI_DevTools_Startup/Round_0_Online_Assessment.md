# Round 0 — Online Assessment
**Interview:** AI / Dev-Tools Startup
**Format:** Auto-graded (HackerRank or similar) — 90 minutes, no human contact until you clear this.

---

## DSA Problems

### Problem 1 — Group Anagrams
**Difficulty:** Medium

Given an array of strings, group the anagrams together. Return a list of groups. The order within each group and the order of groups doesn't matter.

**What I'm testing:**
- Do you immediately reach for a sorted-string as the hash key?
- Can you articulate the time complexity: O(n * k log k) where k is max string length?
- Do you try a character-frequency array as the key for O(n * k) instead? That's the follow-up if you go the sort route.

**Common mistakes to avoid:**
- Treating `"eat"` and `"tea"` as different — sort both first
- Using a list as a dictionary key in Python (not hashable) — use a tuple

---

### Problem 2 — Word Break II
**Difficulty:** Hard

Given a string `s` and a dictionary of strings `wordDict`, add spaces to `s` to construct all possible sentences where each word is a valid dictionary word. Return all such possible sentences.

**What I'm testing:**
- Do you recognize this is DP + backtracking (not pure DP)?
- Do you add memoization to avoid recomputing from the same index?
- Do you handle the edge case where no valid segmentation exists (return empty list, not crash)?
- Can you articulate *why* this blows up exponentially without memoization?

**Example:**
```
s = "catsanddog"
wordDict = ["cat","cats","and","sand","dog"]
Output: ["cats and dog", "cat sand dog"]
```

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
