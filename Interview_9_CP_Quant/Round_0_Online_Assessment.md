# Round 0 — Online Assessment
**Interview:** Competitive-Programming-Heavy / Quant-Adjacent Company
**Format:** Auto-graded — 75 minutes. Tight time limit. The difficulty is closer to a Codeforces Div 2 than a standard OA.

---

## DSA Problems

### Problem 1 — Count of Smaller Numbers After Self
**Difficulty:** Hard

Given an integer array nums, return an integer array counts where counts[i] is the number of smaller elements to the right of nums[i].

---

### Problem 2 — Reverse Pairs
**Difficulty:** Hard

Given an integer array nums, return the number of reverse pairs in the array. A reverse pair is a pair (i, j) where 0 <= i < j < nums.length and nums[i] > 2 * nums[j].

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
