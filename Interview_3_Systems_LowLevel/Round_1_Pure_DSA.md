# Round 1 — Pure DSA
**Interview:** Systems / Low-Level Deep-Dive Company
**Duration:** 60 minutes
**Interviewer's Note:** You reimplemented Git. That means you've worked with commit DAGs, content-addressed storage, and tree serialization. My problem choices are not accidental.

---

## Problem 1 — Serialize and Deserialize a Binary Tree
**Difficulty:** Medium (for you, given your background)
**Time Budget:** 20 minutes

### The Problem
Serialize a binary tree to a string and deserialize it back to the original structure. The serialized format must be reversible.

**Before you code:**
- What traversal order makes deserialization unambiguous? (Pre-order with null markers)
- In your gitlight project, how do you encode a tree object? Is there a parallel?
- What delimiter will you use, and does it conflict with any possible node value?

**Expected approach:**
```
Serialize: pre-order traversal. Null nodes → "null". Values separated by ",".
Example: [1,2,3] → "1,2,null,null,3,null,null"

Deserialize: consume tokens from a queue, recursively rebuild.
```

---

## Problem 2 — Merge Base of a Commit DAG (Custom Problem)
**Difficulty:** Hard
**Time Budget:** 35 minutes

### The Problem
> In a version control system, each commit has one or more parent commits (merge commits have 2+ parents). Given two commit IDs, find their **merge base** — the most recent common ancestor.

This is exactly what `git merge-base A B` computes. And it's directly what your gitlight DAG traversal should have prepared you to think about.

```
Input:
commits = {
  "A": ["C"],
  "B": ["C", "D"],
  "C": ["E"],
  "D": ["E"],
  "E": []
}
Find merge base of "A" and "B"

Output: "C" (most recent common ancestor)
```

**Before you code:**
- Is this a tree or a DAG? (DAG — a commit can have multiple parents, and multiple commits can share parents)
- Does "most recent" mean closest in graph distance, or is there a topological ordering?
- Can you use a BFS from each node and find the first intersection? (Yes — but BFS level doesn't necessarily equal topological depth)
- Better approach: BFS/DFS from both nodes, collect all ancestors of each, find the common ancestor with the greatest topological depth (commit time ordering)

**What I'm grading:**
| Criteria | Weight |
|---|---|
| Recognized it as DAG problem, not binary tree LCA | High |
| Did NOT apply binary tree LCA algorithms directly (they don't work here — no single parent) | High |
| Clarified: "Are there cycles? Can I assume a valid DAG?" | High |
| Correct BFS/DFS ancestor set intersection | Medium |
| Handles disconnected DAGs (no common ancestor) | Medium |

---

## Mid-Solve Twist

*I will say this after you find the single merge base:*

> "Good. Now there are two valid merge bases — the commit graph is diamond-shaped. Commits A and B both descend from C and D, and C and D have no ancestor-descendant relationship. Return all merge bases. Then explain what this means for a 3-way merge."

**What I want:**
- Return ALL ancestors that are common and have no common-ancestor descendants (the "minimal" common ancestors)
- In a 3-way merge context: multiple merge bases means `git` has to do a "recursive merge" — it first merges the two merge bases into a virtual commit, then uses that as the base. This is why `git merge --strategy=recursive` exists.
- You don't need to implement this. I want to know you've thought about it.
