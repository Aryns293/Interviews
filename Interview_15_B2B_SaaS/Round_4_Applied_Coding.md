# Round 4 — Applied Coding: Backend + Full-Stack
**Interview:** B2B SaaS / Enterprise Software
**Duration:** 60 minutes

---

## Build 1 — Idempotent API Middleware (30 minutes)

### The Problem
Write an Express middleware that guarantees an API endpoint is idempotent. If a client sends the same `Idempotency-Key` header twice, return the exact same response as the first time, without executing the route handler again.

```js
function idempotencyMiddleware(req, res, next) {
  const key = req.headers['idempotency-key'];
  if (!key) return next();
  
  // TODO: Implement idempotency check and response caching
}
```

**What I'm watching for:**
- Check Redis/DB for the key.
- If it exists and status is "completed", return the cached response (JSON and Status Code).
- If it exists and status is "processing", return `409 Conflict` (client is retrying too fast while the first request is still running).
- If it doesn't exist, mark it "processing" in Redis, override `res.send` or `res.json` to capture the output, and call `next()`. When `res.send` fires, save the output to Redis and mark as "completed".

---

## Debug 1 — The N+1 Query Problem (15 minutes)

### The Problem
"This GraphQL resolver (or REST endpoint) fetches a list of 100 Companies, and then fetches the Owner for each company. Our DB CPU is at 100%. Fix it."

```js
const companies = await db.query('SELECT * FROM companies LIMIT 100');
for (const company of companies) {
  company.owner = await db.query('SELECT * FROM users WHERE id = $1', [company.owner_id]);
}
```

**The Fix:**
- Classic N+1 problem. 1 query for companies + 100 queries for owners = 101 queries.
- Fix: Use a `WHERE IN` clause or an SQL `JOIN`.
- Fetch companies. Pluck owner IDs into an array. `SELECT * FROM users WHERE id IN (...)`. Map the results back to the companies in memory in Node.js.

---

## Discussion — API Versioning (15 minutes)

> "We are changing the schema of our `Customer` object. We have 10,000 businesses using our API. How do we roll this out?"

*Expected:*
- You cannot break the existing API.
- Create a new version (e.g., `v2` in the URL `/api/v2/customers`, or via headers `Stripe-Version: 2023-08-01`).
- Maintain the old version. Under the hood, the `v1` endpoint calls the new `v2` internal logic, but passes the result through a "response transformer" that mutates the JSON back into the old `v1` shape before sending it to the client.
