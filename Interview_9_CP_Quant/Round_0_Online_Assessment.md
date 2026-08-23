# Round 0 — Online Assessment
**Interview:** Competitive-Programming-Heavy / Quant-Adjacent Company
**Format:** Auto-graded — 75 minutes. Tight time limit. The difficulty is closer to a Codeforces Div 2 than a standard OA.

---

## DSA Problems

### Problem 1 — Maximum XOR of Two Numbers in an Array
**Difficulty:** Medium-Hard

Given an integer array `nums`, return the maximum result of `nums[i] XOR nums[j]`, where `0 <= i <= j < n`.

**What I'm testing:**
- Can you identify that O(n²) brute force will TLE?
- Do you immediately see the bitwise Trie approach?
- Insert all numbers into a Trie (binary tree where left = bit 0, right = bit 1). For each number, walk the Trie trying to pick the opposite bit at each step (to maximize XOR).
- Time: O(N * L) where L is max bits (32).

---

### Problem 2 — Longest Valid Parentheses
**Difficulty:** Hard

Given a string containing just the characters `'('` and `')'`, return the length of the longest valid (well-formed) parentheses substring.

**What I'm testing:**
- Do you know the O(n) Stack approach (push indices) OR the O(n) DP approach?
- Stack approach: push `-1` initially. For `(`, push index. For `)`, pop. If empty, push index (new base). Else, `ans = max(ans, i - stack.top())`.
- O(1) space approach (left/right counters scanning both directions) is bonus points.

---

## MCQs

### Bitwise Operation Tricky Prediction
```c
int a = 10, b = 20;
a ^= b ^= a ^= b;
printf("%d %d", a, b); // 0 10 (Undefined behavior in C/C++, but in JS/Java evaluates left to right. Know the language quirk).
```
*Note: The classic XOR swap `x ^= y; y ^= x; x ^= y;` is safe. The inline version `a ^= b ^= a ^= b` is UB in C++ due to unsequenced modifications.*

### Advanced Complexity Analysis
What is the time complexity of building a Segment Tree vs a Fenwick tree?
- Segment Tree: O(N)
- Fenwick Tree: O(N log N) using standard point updates, but O(N) if built bottom-up.
