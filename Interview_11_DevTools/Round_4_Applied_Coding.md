# Round 4 — Applied Coding: Backend + Full-Stack
**Interview:** Developer Productivity / Tooling Company
**Duration:** 60–90 minutes

---

## Build 1 — Live Custom File Watcher (30 minutes)

### The Problem
Implement a robust file watcher (like `nodemon` or Webpack's watcher) that monitors a directory for changes and triggers a callback. 

*Constraint:* Node's built-in `fs.watch` fires multiple times for a single file save (due to how OS editors write files — truncating, writing, closing). You must debounce these events so the callback only fires once per actual user save.

```js
const fs = require('fs');

function watchDirectoryDebounced(dirPath, callback) {
  // TODO: implement watching with debounce logic
}
```

**What I'm watching for:**
- Correct use of `setTimeout` and `clearTimeout` keyed by the filename.
- Managing a Map of `filename -> timerId`.
- Understanding *why* editors cause multiple events (vim swaps, temp files).

---

## Debug 1 — Collaborative Text Desync (15 minutes)

### The Problem
"We have a basic WebSocket text editor. User A types 'Hello'. User B types 'World'. The server broadcasts literally: `insert(0, 'Hello')` then `insert(0, 'World')`. User A sees 'WorldHello', User B sees 'HelloWorld'. Why?"

**The Fix / Discussion:**
- Network latency causes messages to arrive out of order.
- To fix it without full OT/CRDTs: The server must act as the source of truth, maintaining a sequence number (version). Clients send `(version, operation)`. If a client sends a stale version, the server rejects it and forces the client to pull the latest state, rebase their local change, and resubmit.

---

## Build 2 — A Simple Diff Tool (20 minutes)

### The Problem
Write a function that takes two strings (old text, new text) and returns a list of operations to turn the old text into the new text. (You can assume they are small enough for O(N*M) DP).

*Alternatively (if time is short):* Just explain the Longest Common Subsequence (LCS) approach you used in `gitlight`.

**What I'm watching for:**
- Constructing the 2D DP table.
- The crucial part: backtracking through the table to reconstruct the actual Diff operations (Insert X, Delete Y, Keep Z), not just returning the length of the LCS.
