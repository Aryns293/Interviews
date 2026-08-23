# Round 0 — Online Assessment
**Interview:** General Product-Based Company (Balanced Loop)
**Format:** Auto-graded — 90 minutes.

---

## DSA Problems

### Problem 1 — Top K Frequent Elements
**Difficulty:** Medium

Given an array of integers, return the `k` most frequent elements.

**What I'm testing:**
- Min-heap of size k: O(n log k)
- Bucket sort: O(n) — can you think of this?
- Do you handle ties correctly?

### Problem 2 — Median of Two Sorted Arrays
**Difficulty:** Hard

Find the median of two sorted arrays in O(log(min(m,n))) time.

**What I'm testing:**
- Do you know the binary search on the smaller array approach?
- Do you handle even/odd total length?
- Do you start with the O(m+n) merge and then push for optimization?

---

## MCQs

### DBMS — Normal Form Identification
Given:
```
R(A, B, C, D) with FDs: A→B, B→C, A→D
```
- Is this in 2NF? 3NF? BCNF?
- Work through it step by step.

### Java/C++ — Bug Finding
```java
List<String> list = new ArrayList<>();
for (String s : list) {
    if (s.equals("remove")) {
        list.remove(s); // Bug?
    }
}
```
**Answer:** Yes — `ConcurrentModificationException`. You cannot remove from a collection while iterating over it with an enhanced for loop. Use `Iterator.remove()` or collect items to remove separately.

### HTTP Status Codes
- 200 vs 201 vs 204
- 400 vs 422 vs 409
- 500 vs 502 vs 503
