# Round 4 — Applied Coding: Backend + Full-Stack
**Interview:** General Product-Based Company
**Duration:** 60–90 minutes

---

## Build 1 — REST CRUD API with JWT Auth (30 minutes)

### The Problem
Build a minimal REST API for a to-do application. Protect the CRUD routes with JWT auth middleware.

**Endpoints:**
```
POST   /auth/register   → create user, return JWT
POST   /auth/login      → validate credentials, return JWT
GET    /todos           → list all todos for the authenticated user
POST   /todos           → create a new todo
PATCH  /todos/:id       → update a todo (only owner can update)
DELETE /todos/:id       → delete a todo (only owner can delete)
```

**Scaffold provided:**
```js
const express = require('express');
const jwt = require('jsonwebtoken');
const bcrypt = require('bcrypt');

const app = express();
app.use(express.json());

// In-memory stores (no DB for this exercise)
const users = [];
const todos = [];

const JWT_SECRET = 'interview-secret';

// Build auth middleware and all routes
```

**What I'm watching for:**
- Does your middleware extract the token from `Authorization: Bearer <token>` correctly?
- Do you use `jwt.verify()`, not just `jwt.decode()`?
- Do you hash passwords with bcrypt before storing?
- Do you check ownership (todo.userId === req.user.id) on PATCH and DELETE?
- Correct HTTP status codes: 201 for creation, 401 for missing auth, 403 for wrong user, 404 for not found

---

## Build 2 — Redis Cache-Aside for Job Status (15 minutes)

### The Problem
Add a Redis cache-aside layer to a "get job status" endpoint. Read from cache on hit, fall through to Postgres on miss, write to cache on miss.

```js
app.get('/jobs/:id/status', async (req, res) => {
  const { id } = req.params;

  // TODO: Cache-aside pattern
  // 1. Check Redis for key `job:status:<id>`
  // 2. On hit: return cached value
  // 3. On miss: query Postgres, store in Redis with 60s TTL, return value
});
```

**Follow-up:** When would you NOT cache this endpoint?
*Expected:* If job status changes frequently (e.g., real-time status updates), a 60s TTL means stale data. Options: shorter TTL, event-driven cache invalidation (invalidate when status changes), or WebSocket push instead of polling.

---

## Debug 1 — DB Connection Leak (15 minutes)

### The Problem
This endpoint causes a DB connection leak under load. Find and fix it.

```js
app.get('/users/:id', async (req, res) => {
  const client = await pool.connect();
  try {
    const result = await client.query('SELECT * FROM users WHERE id = $1', [req.params.id]);
    res.json(result.rows[0]);
  } catch (err) {
    res.status(500).json({ error: err.message });
    // Bug is here
  }
});
```

**The bug:** When an error is thrown, the code returns a 500 response but never calls `client.release()`. The connection is checked out of the pool forever.

**Fix:** Add `client.release()` in a `finally` block — it runs whether the try block succeeds or fails.

---

## Discussion — useMemo vs useCallback (10 minutes)

**Q:** When does `useMemo` actually prevent a re-render, and when does it just add overhead?

*Expected:*
- `useMemo` prevents recomputing an expensive value on every render — it doesn't prevent re-renders itself
- `useCallback` prevents a function from being recreated on every render
- These only matter when passed as props to a child wrapped in `React.memo` — without `React.memo` on the child, the function reference changing doesn't matter
- **When they add overhead for no benefit:** Memoizing a cheap computation or a primitive value (string, number) — the memoization overhead exceeds the computation cost
