# Round 4 — Applied Coding: Backend + Full-Stack
**Interview:** Fintech / Payments Company
**Duration:** 60–90 minutes

---

## Build 1 — Idempotency-Key Middleware With Concurrent Safety (30 minutes)

### The Problem
Build an Express middleware for an idempotent POST endpoint. The client sends an `Idempotency-Key` header. Handle concurrent requests with the same key atomically.

**Requirements:**
1. On first request with a key: process, store `{status, response}` in Redis with 24h TTL, return response
2. On retry with same key (key exists, status `completed`): return cached response immediately — no re-processing
3. On concurrent request with same key (key exists, status `processing`): return `409 Conflict` — tell the client to retry after the first one completes
4. Missing key: return `400 Bad Request`

```js
const idempotencyMiddleware = async (req, res, next) => {
  const key = req.headers['idempotency-key'];
  if (!key) return res.status(400).json({ error: 'Idempotency-Key header required' });

  const redisKey = `idempotency:${key}`;

  // TODO: atomic check-and-set using SET NX
  // If key exists and status === 'completed' → return cached response
  // If key exists and status === 'processing' → return 409
  // If key doesn't exist → set status 'processing', call next(), then update to 'completed'
};
```

**The atomic trick I'm watching for:**
```js
// WRONG — race condition: two requests both GET null, both proceed
const existing = await redis.get(redisKey);
if (!existing) { /* process */ }

// CORRECT — SET NX is atomic: only one caller gets 'OK', all others get null
const acquired = await redis.set(redisKey, JSON.stringify({status:'processing'}), {NX: true, EX: 86400});
if (!acquired) { /* key already exists, check status */ }
```

---

## Build 2 — Atomic Debit/Credit Ledger Transfer (20 minutes)

### The Problem
Implement a `transfer(fromAccountId, toAccountId, amount)` function that atomically debits one account and credits another. Either both happen or neither does. Prevent overdrafts.

```js
async function transfer(fromId, toId, amount, db) {
  // TODO: wrap in a DB transaction
  // 1. Lock both accounts (SELECT FOR UPDATE in canonical order to prevent deadlock)
  // 2. Check fromAccount.balance >= amount (prevent overdraft)
  // 3. Debit fromAccount, credit toAccount
  // 4. Insert audit record into transactions table
  // 5. COMMIT — or ROLLBACK on any failure
}
```

**Deadlock prevention I'm watching for:**
- Always lock accounts in ascending ID order: `MIN(fromId, toId)` first
- This prevents the circular wait that causes deadlock when two transfers run simultaneously in opposite directions

**What I'm grading:**
| Criteria | Weight |
|---|---|
| Correct transaction wrapping with rollback on error | High |
| Overdraft check inside the transaction (not before) | High |
| Lock ordering to prevent deadlock | High |
| Audit record insertion inside the same transaction | Medium |

---

## Live Debug — N+1 in Transaction History (15 minutes)

```js
// Broken
app.get('/users/:id/transactions', async (req, res) => {
  const txns = await db.query('SELECT * FROM transactions WHERE user_id = $1', [req.params.id]);
  const result = await Promise.all(txns.rows.map(async (t) => {
    const merchant = await db.query('SELECT name FROM merchants WHERE id = $1', [t.merchant_id]);
    return { ...t, merchant_name: merchant.rows[0].name };
  }));
  res.json(result);
});
```

Fix with a JOIN. Then: "How many queries does this make for a user with 500 transactions? What's the Postgres connection pool impact?"

---

## Discussion — Never Trust Client-Side Payment Validation (10 minutes)

**Q:** "A junior dev suggests validating the payment amount in the React frontend before hitting the server — 'to give faster feedback.' Walk me through the actual exploit if the server trusts that client-side check."

*Expected:* Any client-side validation can be bypassed by:
1. Using `curl` or Postman to bypass the browser entirely
2. Using browser DevTools to modify the JavaScript before it runs
3. Intercepting the request with a proxy (Burp Suite) and modifying the payload in flight

*The rule:* Client-side validation is UX. Server-side validation is security. They serve different purposes. The server MUST re-validate every input regardless of what the client says.

*Fintech specific:* If the server trusts `amount: 0.01` from the client for a ₹10,000 purchase, an attacker pays ₹0.01 for anything.
