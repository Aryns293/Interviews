# Round 2 — CS Fundamentals
**Interview:** Real-Time Communications / Streaming
**Duration:** 45 minutes
**Theme:** Protocols, WebSockets, Networking, CodeSync AI's real-time nature.

---

## Networking — WebRTC vs WebSockets

**Q1:** "CodeSync AI uses WebSockets for real-time code editing. If you were building Zoom (video), would you use WebSockets? Why or why not?"
*Expected:* No. WebSockets are built on TCP. TCP guarantees delivery by forcing retransmissions of lost packets, which stalls the entire stream (Head-of-Line Blocking). For video, you use WebRTC (which uses UDP under the hood) because dropping a frame is better than freezing the video for 2 seconds waiting for a retransmission.

**Q2:** "What is a STUN / TURN server?"
*Expected:* In P2P networking (WebRTC), NAT (Network Address Translation) prevents devices from knowing their own public IP address. STUN servers help a device discover its public IP. If strict firewalls block P2P, a TURN server acts as a centralized relay to bounce the traffic.

---

## Operating Systems — The C10K Problem

**Q:** "A single Node.js instance of CodeSync AI is handling 10,000 concurrent WebSocket connections. Why doesn't the OS run out of RAM if a traditional thread-per-connection server (like old Apache or Tomcat) would crash?"
*Expected:* Thread-per-connection allocates a dedicated OS thread (and an 8MB stack) per user. 10k users = 80GB RAM just for idle threads. Node.js (and Nginx, Redis) uses an asynchronous, non-blocking I/O model (epoll/kqueue) with a single event loop. An idle connection costs almost zero CPU and only a few KB of memory.

---

## Databases — Real-Time Presence

**Q:** "How would you store 'Online/Offline' status for millions of users (like Discord's green dot)?"
*Expected:*
- Do NOT use Postgres/MySQL for this. It is highly ephemeral, high-write data.
- Use Redis. Specifically, Redis Sets or Hash Maps for mapping `user_id -> node_id`, and Pub/Sub to broadcast state changes.
- To handle a server crashing without users sending "offline" events, use Redis key expiration (TTL) acting as a heartbeat. If the client stops pinging every 30 seconds, the key expires, and a keyspace notification triggers an "Offline" broadcast.


---

## Exhaustive CS Fundamentals (Theoretical & SQL)
*These are additional deep-dive topics specifically assigned to this interview loop to ensure 100% breadth coverage across all 20 interviews.*

### OS - File Systems
**Inodes:** What is an inode in Unix-like systems?

### CN - Web Arch
**DNS (Domain Name System):** Explain the difference between an Iterative and Recursive DNS query.

### SQL - Query 4
**Self-Joins & Hierarchies:** Write a query to find the names of employees who earn strictly more than their direct managers.



---

## Master Question Bank — Assigned Slice (Round 14)
*These questions are your assigned coverage from the 275-question Master Bank. Every question appears exactly once across all 20 interviews.*

### Operating Systems (OS)
- Kernel mode vs user mode
- Spinlock vs mutex — when would you use each?

### Database Management Systems (DBMS)
- MVCC (Multi-Version Concurrency Control)
- DELETE vs TRUNCATE vs DROP

### SQL — Practical Query Problems
- CASE WHEN inside a SELECT

### Computer Networks (CN)
- Firewall — how does it filter traffic?
- HTTP vs HTTPS at a mechanical level

### Object-Oriented Programming (OOP)
- Runtime vs compile-time polymorphism

### Linux
- tail -f — when is it used?

### Security
- Non-root container user — what does it protect against?

### Git
- git blame

### Language Internals — Java
- volatile keyword

### Language Internals — C++
- Pure virtual function — what makes a class abstract

