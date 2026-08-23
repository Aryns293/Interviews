# Round 2 — CS Fundamentals
**Interview:** AI / Dev-Tools Startup
**Duration:** 60–75 minutes
**Theme:** Your stack is Node.js + Socket.IO + Docker + JWT. Every topic I pick maps to something in CodeSync AI or your internship. I'm not asking textbook questions to fill time — I'm asking them to see if you understand what your own code is doing.

---

## Computer Networks — The Full Stack

**Q1 — The classic:**
> "Walk me through exactly what happens, step by step, from the moment I type `google.com` in my browser and press Enter, to the page appearing on my screen."

*Expected sequence:* DNS resolution → TCP 3-way handshake → TLS handshake → HTTP GET request → HTTP response → browser renders HTML → subsequent requests for assets.

*I'll stop you if you skip a step. The TCP handshake and TLS handshake are the most commonly skipped.*

---

**Q2 (CodeSync AI tie-in):**
> "Your CodeSync AI uses Socket.IO. Is that WebSocket the whole way, or does it fall back to something else? When and why does the fallback happen?"

*Expected answer:*
- Socket.IO starts with HTTP long-polling and **upgrades** to WebSocket if the server and client both support it
- The fallback to long-polling happens when WebSocket connections are blocked (corporate firewalls, certain proxies, older browsers)
- This means your first Socket.IO "connection" is actually an HTTP request — the WebSocket upgrade happens shortly after

---

## OOP — Composition in Your Own Project

**Q1:** Walk me through the 4 pillars of OOP with live code examples in JavaScript.

**Q2 (direct project tie-in):**
> "In CodeSync AI, you have two code execution strategies — Docker (primary) and Judge0 (fallback). How did you model that relationship? Did you use inheritance or composition, and why?"

*What I want to hear:*
- Composition ("has-a") is almost always preferred over inheritance ("is-a") for strategy switching
- An `Executor` interface with `execute(code, language)` method
- `DockerExecutor` and `Judge0Executor` both implement `Executor`
- The `ExecutionService` holds a reference to whichever executor is currently active — this is the **Strategy Pattern**
- Inheritance would create a rigid hierarchy; composition lets you swap at runtime or even combine them

---

## JavaScript / Node.js Internals — The Event Loop

**Q1 — Live trace:**
Predict the exact output order of this code. Explain why.

```js
console.log('A');

setTimeout(() => console.log('B'), 0);

Promise.resolve()
  .then(() => console.log('C'))
  .then(() => console.log('D'));

console.log('E');
```

*Expected output:* `A, E, C, D, B`

*Explanation:*
1. `A` and `E` are synchronous — call stack, runs first
2. `.then()` callbacks are microtasks — queued in the microtask queue, drain before the event loop moves to the next macrotask
3. `setTimeout` callback is a macrotask — only runs after all microtasks are drained

---

**Q2 (project tie-in):**
> "Node.js is single-threaded. You're handling multiple concurrent Socket.IO rooms in CodeSync AI on a single process. How does that work — isn't one room's heavy computation blocking the others?"

*Expected answer:*
- Node.js is non-blocking because I/O operations (socket reads, file reads, DB queries) are handled by libuv and the OS, not the JS thread
- The JS event loop only runs callbacks when I/O is complete — it never sits and waits
- **However:** CPU-bound work (like running code synchronously in JS) WOULD block the event loop
- This is exactly why code execution in CodeSync AI runs in a **Docker container** — it's a separate OS process, not blocking the Node.js event loop

---

## Security — JWT & XSS

**Q1:**
> "Walk me through the structure of a JWT. If I decode the payload without verifying the signature, what information do I get, and what risk does that create?"

*Expected answer:*
- JWT = `base64(header).base64(payload).signature`
- Payload is NOT encrypted — anyone can decode it. It's only **signed**.
- Risk: if you trust the payload without verifying the signature, an attacker can modify the payload (change `role: "user"` to `role: "admin"`) and forge a valid-looking JWT
- `jwt.verify()` verifies the signature — never skip this

---

**Q2 (project-specific risk):**
> "CodeSync AI allows users to submit code and see each other's output in a shared room. What's the XSS risk if you render that output as HTML instead of plain text?"

*Expected answer:*
- If a user submits `<script>alert(document.cookie)</script>` as "code" and you render it as innerHTML, it executes in every viewer's browser
- Fix: always render user-submitted content as plain text (`textContent`, not `innerHTML`)
- Additional layer: Content Security Policy (CSP) header that disallows inline scripts


---

## Exhaustive CS Fundamentals (Theoretical & SQL)
*These are additional deep-dive topics specifically assigned to this interview loop to ensure 100% breadth coverage across all 20 interviews.*

### OS - Process Management
**Context Switching:** What happens during a context switch? Why is switching between threads faster than switching between processes?

### DBMS - Normalization
**Anomalies:** What are Insertion, Deletion, and Updation anomalies?

### OOP - 4 Pillars
**Polymorphism:** Explain Compile-time polymorphism (Method Overloading) vs Run-time polymorphism (Method Overriding / Dynamic Dispatch).

