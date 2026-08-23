# Round 4 — Applied Coding: Backend + Full-Stack
**Interview:** Cyber Security / Compliance-Heavy Company
**Duration:** 60 minutes

---

## Build 1 — Secure Webhook Receiver (30 minutes)

### The Problem
Write an Express middleware to securely verify an incoming webhook from GitHub.

```js
const crypto = require('crypto');
const secret = process.env.WEBHOOK_SECRET;

function verifySignature(req, res, next) {
  // TODO: implement HMAC-SHA256 verification
}
```

**What I'm watching for:**
- Do you read the raw body? (Express `req.body` is usually parsed JSON. HMAC requires the exact raw byte string sent by the client. You must use `express.raw({type: 'application/json'})` or capture it in a `verify` callback).
- Do you use `crypto.createHmac('sha256', secret).update(rawBody).digest('hex')`?
- **Crucial:** Do you use `crypto.timingSafeEqual()` to compare the signatures instead of `===`?

---

## Debug 1 — The JWT Vulnerability (15 minutes)

### The Problem
```js
const jwt = require('jsonwebtoken');

app.post('/api/data', (req, res) => {
  const token = req.headers.authorization.split(' ')[1];
  
  // Find the bug
  const decoded = jwt.decode(token);
  if (decoded && decoded.role === 'admin') {
    res.json(adminData);
  } else {
    res.status(403).send('Forbidden');
  }
});
```

**The Fix:**
- `jwt.decode()` does NOT verify the signature. It just base64-decodes the payload. An attacker can change their role to 'admin', re-encode it, and bypass the check.
- Fix: Must use `jwt.verify(token, SECRET_KEY)`.

---

## Discussion — SQL Injection vs NoSQL Injection (15 minutes)

> "You use MongoDB in CodeSync AI. Is MongoDB immune to injection attacks because it doesn't use SQL strings?"

*Expected:*
- No. NoSQL injection is real.
- Example: `db.users.find({ username: req.body.username, password: req.body.password })`.
- If an attacker sends `{ "username": "admin", "password": { "$ne": null } }` (as a parsed JSON object, not a string), the query becomes `find where username is admin and password is not null`, which evaluates to true and bypasses authentication.
- Fix: Always cast inputs to strings or sanitize JSON payloads before passing them to Mongo drivers.
