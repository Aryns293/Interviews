# Round 4 — Applied Coding: Backend + Full-Stack
**Interview:** Systems / Low-Level Deep-Dive Company
**Duration:** 60–90 minutes

---

## Build 1 — Minimal Content-Addressable Store (30 minutes)

### The Problem
> Implement a minimal content-addressable object store in Node.js. Given arbitrary content, hash it with SHA-256, store it at a path derived from the hash, and retrieve it by hash.

This directly reproduces the core of gitlight. No notes. Build it from memory.

```js
const crypto = require('crypto');
const fs = require('fs');
const path = require('path');

const STORE_DIR = './.object_store';

// hash content and store it — return the hex hash
function store(content) {
  // TODO
}

// retrieve content by its hash — return the raw content or null
function retrieve(hash) {
  // TODO
}
```

**Requirements:**
1. Hash: `SHA-256(content)` → 64-char hex string
2. Path: `STORE_DIR/<first-2-chars>/<remaining-62-chars>`
3. Create intermediate directories if they don't exist
4. `retrieve(hash)` returns null if no object found
5. Two different pieces of content with the same hash (SHA-256 collision) would conflict — acknowledge this but don't over-engineer it

**What I'm watching for:**
- Do you split the hash correctly for the directory structure (2/62, same as Git's 2/38 for SHA-1)?
- Do you use `fs.mkdirSync({ recursive: true })` to avoid "directory not found" errors?
- Do you handle binary content correctly (Buffer vs string)?
- Can you explain why content-addressing naturally deduplicates identical files?

---

## Build 2 — Rate-Limiting Middleware (20 minutes)

### The Problem
Build Express middleware that rate-limits requests to 100 requests per minute per IP address, using a sliding window counter stored in memory.

```js
const express = require('express');
const app = express();

// Implement this
const rateLimiter = (req, res, next) => {
  // TODO
};

app.use(rateLimiter);
```

**Follow-up:** Your in-memory store resets when the server restarts. How would you move the counter to Redis? What Redis data structure and which commands would you use? (Hint: ZADD with timestamps for a sliding window log, or INCR + EXPIRE for a fixed window counter)

---

## Discussion — Event Loop Trace (10 minutes)

**Q:** Predict the exact output order of this Node.js script. Explain why.

```js
const fs = require('fs');

fs.readFile('./test.txt', 'utf8', () => {
  console.log('A - file read callback');
  setTimeout(() => console.log('B - timeout inside callback'), 0);
  Promise.resolve().then(() => console.log('C - microtask inside callback'));
});

setTimeout(() => console.log('D - top-level timeout'), 0);
Promise.resolve().then(() => console.log('E - top-level microtask'));

console.log('F - synchronous');
```

**Expected output:** `F → E → D → A → C → B`

*Explanation:*
- `F` runs synchronously first
- `E` (microtask) drains before the event loop moves to macrotasks
- `D` (top-level setTimeout) runs as the first macrotask
- `A` (fs callback) runs as a macrotask from the I/O phase
- Inside `A`: `C` (microtask) drains before `B` (macrotask)

---

## Live Debug — N+1 with Raw SQL Twist (15 minutes)

### The Problem
Fix the N+1 query on this endpoint. Then — **without changing the ORM call** — show me how to rewrite it with a raw SQL JOIN instead.

```js
// Broken — N+1
const commits = await Commit.findAll();
for (const commit of commits) {
  commit.author = await User.findByPk(commit.authorId);
}

// Step 1: Fix with eager loading (Sequelize include)
// Step 2: Rewrite entirely with raw SQL using a JOIN
```

**Step 2 expected:**
```sql
SELECT commits.*, users.name AS author_name, users.email AS author_email
FROM commits
JOIN users ON commits.author_id = users.id;
```

**Follow-up:** When would you choose raw SQL over ORM eager loading? (Answer: complex aggregations, cross-table window functions, performance-critical paths where the ORM generates suboptimal queries)
