# Round 4 — Applied Coding: Backend + Full-Stack
**Interview:** Early-Stage Seed Startup
**Duration:** 60–90 minutes
**Style:** Fast-paced live coding. I will interrupt you with changing requirements mid-solve.

---

## Build 1 — 30-Minute MVP Signup Endpoint

### The Problem
Build a signup + JWT-auth endpoint, scoped to "ship in 30 minutes".
Explicitly NO time for refresh-token rotation, rate limiting, or email verification.

```js
const express = require('express');
const bcrypt = require('bcrypt');
const jwt = require('jsonwebtoken');

const app = express();
app.use(express.json());

const users = []; // Mock DB
const JWT_SECRET = 'startup-secret';

app.post('/signup', async (req, res) => {
  // TODO: Implement signup, hash password, return JWT
});
```

**What I'm watching for:**
- Do you check if the user already exists? (Return 409 Conflict)
- Do you await `bcrypt.hash()` correctly?
- Do you sign the JWT with a reasonable expiry (e.g., `expiresIn: '24h'`)?

**The Mid-Solve Twist:**
> "Okay, the founder says we're fundraising next week and a security review is happening. We need to retrofit security into this endpoint right now. What's the FIRST thing you add?"

*Expected answers (in order of priority):*
1. **Rate Limiting:** Protect against brute-force account creation or credential stuffing.
2. **Input Validation:** Ensure email is somewhat valid, password meets complexity (zxcvbn or simple length check) to prevent trivial abuse.
3. **HTTP-Only Cookies:** Instead of returning the JWT in the JSON body (vulnerable to XSS), set it in an `HttpOnly`, `Secure` cookie.

---

## Live Debug — React Component Re-rendering (10 minutes)

### The Problem
This component re-renders the heavy child on every keystroke. Fix it fast.

```jsx
import React, { useState } from 'react';

const HeavyChild = ({ data, onClick }) => {
  console.log("HeavyChild re-rendered!");
  // ... expensive render ...
  return <div onClick={onClick}>Heavy Child {data.id}</div>;
};

export default function App() {
  const [text, setText] = useState("");
  const [data, setData] = useState({ id: 1 });

  const handleClick = () => {
    console.log("Clicked child");
  };

  return (
    <div>
      <input value={text} onChange={e => setText(e.target.value)} />
      <HeavyChild data={data} onClick={handleClick} />
    </div>
  );
}
```

**The Fix:**
1. Wrap `HeavyChild` in `React.memo()`.
2. Wrap `handleClick` in `useCallback` with `[]` deps.
3. Ensure `data` object reference doesn't change on every render (it's in state, so it's fine here, but if it were an inline object literal, it would break memoization).

---

## Discussion — Cache-Aside TTL (5 minutes)

> "You're adding Redis cache-aside for a 'get user profile' endpoint. We have zero traffic data to base a TTL choice on. What TTL do you pick and why?"

*Expected pragmatic answer:*
- Start short — 5 minutes or even 60 seconds.
- Why? It's long enough to absorb a sudden spike of traffic (e.g., someone spamming reload, or a link going briefly viral) preventing DB overload.
- It's short enough that if a user updates their profile, they only see stale data for a max of 5 minutes (if you didn't build cache invalidation on write yet).
- Picking 24 hours without an invalidation strategy guarantees a flood of "my profile update didn't save" support tickets.
