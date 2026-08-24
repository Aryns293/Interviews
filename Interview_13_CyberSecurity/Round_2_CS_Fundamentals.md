# Round 2 — CS Fundamentals
**Interview:** Cyber Security / Compliance-Heavy Company
**Duration:** 60 minutes
**Theme:** Rolewize security deeply audited.

---

## Cryptography — HMAC Deep Dive

**Q1:** "In Rolewize, you used HMAC-SHA256 for webhook verification. Why HMAC and not just a simple hash like `SHA256(secret + payload)`?"
*Expected:* A simple hash is vulnerable to a Length Extension Attack. An attacker who sees `hash(secret + data)` can easily compute `hash(secret + data + extra_data)` without knowing the secret. HMAC uses a specific nested hashing structure (`hash(key XOR opad, hash(key XOR ipad, message))`) which is mathematically immune to length extension attacks.

**Q2:** "You mentioned `crypto.timingSafeEqual()`. Explain the exact mechanism of a timing attack."
*Expected:* If a string comparison function (`==` or `===`) returns `false` on the first mismatched character, an attacker can send thousands of requests and measure the microsecond difference in response times. If the response takes slightly longer, they guessed the first character correctly. They can brute force a 64-character hash in minutes.

---

## Operating Systems — Sandboxing (CodeSync AI)

**Q:** "You run user code in a Docker container. I am a malicious user. I want to escape your container and read the environment variables on the host machine. How would I do it, and how does your architecture prevent it?"
*Expected:*
- Attack vectors: Kernel exploits (e.g., Dirty COW), privileged containers, or exploiting a mounted socket (like `/var/run/docker.sock`).
- Prevention: 
  - Never mount the Docker socket into the container.
  - Run the container as an unprivileged user (`USER node` in Dockerfile).
  - Use `ulimit` and cgroups to prevent fork bombs and memory exhaustion.
  - Ideally, run untrusted code in a gVisor sandbox or Firecracker microVM, because standard Docker namespaces share the underlying host kernel.

---

## IAM and RBAC

**Q:** "Rolewize processes data for multiple companies. How do you ensure that Company A cannot query QueueFlow to see Company B's webhooks?"
*Expected:* 
- Database level: Every row in the DB must have a `tenant_id`. Every query must include `WHERE tenant_id = ?`.
- Better: Row-Level Security (RLS) in Postgres, which enforces the `tenant_id` check at the database layer so a developer can't accidentally omit the `WHERE` clause.
- Queue level: Use distinct Redis namespaces or separate logical databases (`SELECT 1`) for different tenants, or prefix every key with `tenant_id:`.


---

## Exhaustive CS Fundamentals (Theoretical & SQL)
*These are additional deep-dive topics specifically assigned to this interview loop to ensure 100% breadth coverage across all 20 interviews.*

### OS - Memory Management
**Thrashing & Replacement:** What causes a system to thrash, and how does the OS mitigate it? How does LRU work?

### CN - Web Arch
**The 'Google' Question:** What happens from the moment you type `https://google.com` to when the page renders? (DNS, ARP, TCP, TLS, HTTP, DOM Parsing).

### SQL - Query 3
**Cumulative Sum (Running Total):** Calculate the running total of revenue day by day using `SUM() OVER(ORDER BY ...)`.



---

## Master Question Bank — Assigned Slice (Round 13)
*These questions are your assigned coverage from the 275-question Master Bank. Every question appears exactly once across all 20 interviews.*

### Operating Systems (OS)
- Inter-process communication mechanisms (pipes, message queues, shared memory, sockets)
- What is a system call? Give examples

### Database Management Systems (DBMS)
- Deadlock-free locking protocols
- Optimistic vs pessimistic concurrency control

### SQL — Practical Query Problems
- CHAR vs VARCHAR

### Computer Networks (CN)
- REST vs GraphQL
- CORS — why does the browser enforce it?

### Object-Oriented Programming (OOP)
- Static method/variable vs instance method/variable

### Linux
- Shell script vs compiled binary

### Security
- Principle of least privilege

### Git
- git bisect

### Language Internals — Java
- ArrayList vs LinkedList — time complexity tradeoffs

### Language Internals — C++
- Name mangling

