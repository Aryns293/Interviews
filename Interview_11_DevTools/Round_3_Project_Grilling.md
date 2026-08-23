# Round 3 — Past Experience & Project Grilling
**Interview:** Developer Productivity / Tooling Company
**Duration:** 45 minutes
**Project in focus:** `gitlight` and `CodeSync AI`.

> **Interviewer's mindset:** You built developer tools. DevTools have zero tolerance for data loss and require high performance because developers are impatient users. I'm going to push on exactly how deeply you understand the tools you mimic.

---

## Q1 — gitlight: Content Addressing

> "You implemented `gitlight`, essentially rebuilding Git's plumbing layer in Node.js. Explain how Git ensures data integrity using SHA-1. If two different files somehow produce the same SHA-1 hash (a collision), what happens to your gitlight implementation?"

*What I'm listening for:*
- Understanding that Git is a content-addressable file system. The key is the hash of the value.
- If a collision occurs (which is theoretically possible, e.g., SHAttered attack), Git (and gitlight) will assume the new file is identical to the old file because the hash is the same. It will NOT store the new content, resulting in data loss for the new file.
- (Bonus: Git has begun migrating to SHA-256 to mitigate this).

---

## Q2 — gitlight: The DAG & LCA

> "You implemented a custom Lowest Common Ancestor (LCA) algorithm to find the merge base of two branches in `gitlight`. Walk me through how you traverse a Directed Acyclic Graph of commits to find the LCA. What if there are multiple valid merge bases?"

*Expected:*
- Start from both commits, traverse parent pointers backward.
- Can use BFS or DFS, marking visited nodes (or tracking them in a Set). The first intersection point is a common ancestor.
- Multiple merge bases occur in complex cross-merges (criss-cross merges). Git uses recursive merging to handle this. If your `gitlight` just picks the first one it finds, acknowledge that as a known limitation of your simplified implementation.

---

## Q3 — CodeSync AI: Docker Escape Risk

> "You allow users to execute arbitrary code in a Docker sandbox in CodeSync AI. What prevents a user from writing a C++ program that exploits a kernel vulnerability, escapes the Docker container, and gets root access to your Render host?"

*What I want to hear:*
- Honesty: "A standard Docker container is not a true security boundary against a determined attacker with a kernel zero-day."
- Mitigations you *should* use: Run the container as non-root (User Namespaces), drop all Linux capabilities (`--cap-drop=ALL`), use a read-only root filesystem, and limit network egress.
- If you didn't do these things in your project, say so! "For the MVP, I relied on Docker's default isolation, but for a production execution engine, I would use gVisor or Firecracker microVMs."

---

## Q4 — The "Why" Question

> "Why rebuild Git? Why build a collaborative IDE? Both are solved problems with massive engineering teams behind them. What did you learn building them that you couldn't learn by just using them?"

*Expected:* Passion for deep understanding. "I know how to use `git commit`, but I wanted to understand how it actually calculates the diff and stores the tree. You can't truly understand version control until you try to write a recursive tree-diffing algorithm."
