# Round 1 — Pure DSA
**Interview:** E-Commerce / High-Traffic Retail
**Duration:** 60 minutes
**Interviewer's Note:** I want to see you optimize for O(1) space or heavily optimize time complexity. E-commerce operates at massive scale; constant factors matter.

---

## Problem 1 — Word Break
**Difficulty:** Medium
**Time Budget:** 25 minutes

### The Problem
Given a string `s` and a dictionary of strings `wordDict`, return true if `s` can be segmented into a space-separated sequence of dictionary words.

**What I want to see:**
- You immediately identify DP.
- You initialize a boolean array of size `s.length + 1`.
- Clean loops. 
- *Optimization bonus:* Instead of checking all `j` from `0` to `i`, only check `j` from `i - maxWordLength` to `i`. This changes the inner loop from O(N) to O(K) where K is the max length of a dictionary word. I will specifically look to see if you mention this optimization.

---

## Problem 2 — Serialize and Deserialize Binary Tree
**Difficulty:** Hard
**Time Budget:** 35 minutes

### The Problem
Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work.

**E-Commerce framing:** Storing a complex nested category tree (Electronics -> Phones -> Apple -> iPhone 15) in Redis efficiently.

**What I want to see:**
- Pre-order traversal (DFS) is usually the easiest to write for this.
- Serializing: `root.val + "," + serialize(left) + "," + serialize(right)`. Nulls become `"X"`.
- Deserializing: Keep a global or pointer index. Read the array of split strings. If `"X"`, return null. Else, create Node, recursively call for left and right.
- You must handle negative numbers and multi-digit numbers (don't assume `root.val` is a single character).

**Mid-Solve Twist:**
> "The tree is extremely wide but not very deep (like a mega-menu). Would BFS (level-order) be better than DFS for serialization?"

*Expected:* BFS is often better for wide, shallow trees if you want to stream the tree to the client and render the top levels first (progressive rendering). DFS requires parsing deep into one branch before seeing the siblings at level 2.
