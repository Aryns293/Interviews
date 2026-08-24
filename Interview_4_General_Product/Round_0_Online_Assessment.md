# Round 0 — Online Assessment
**Interview:** General Product-Based Company (Balanced Loop)
**Format:** Auto-graded — 90 minutes.

---

## DSA Problems

### Problem 1 — Largest Rectangle in Histogram
**Difficulty:** Hard

Given an array of integers heights representing the histogram's bar height where the width of each bar is 1, return the area of the largest rectangle in the histogram.

---

### Problem 2 — Longest Valid Parentheses
**Difficulty:** Hard

Given a string containing just the characters '(' and ')' return the length of the longest valid (well-formed) parentheses substring.

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
