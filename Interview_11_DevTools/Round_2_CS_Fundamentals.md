# Round 2 — CS Fundamentals
**Interview:** Developer Productivity / Tooling Company
**Duration:** 60 minutes
**Theme:** Under the hood of developer environments. Git internals, OS file descriptors, and Node.js process streams.

---

## Git Internals (Heavy on gitlight)

**Q1:** "You built `gitlight`. Explain the difference between a `blob`, a `tree`, and a `commit` object."
*Expected:* 
- Blob: stores file content only (no filename). 
- Tree: maps filenames to blob hashes (like a directory). 
- Commit: points to a root tree, parent commit(s), and author metadata.

**Q2:** "If I change exactly one character in a 10MB file and commit it, what does Git store under the hood?"
*Expected:* Git stores a brand new 10MB blob object with a new SHA-1 hash (loose object). It does *not* just store the diff initially. Only later, during garbage collection (`git gc`), does Git pack objects into packfiles using delta compression to save space.

---

## Operating Systems — File Descriptors & Streams

**Q1:** "What happens at the OS level when a Node.js script opens 10,000 files simultaneously?"
*Expected:* It will crash with `EMFILE (Too many open files)`. The OS has a limit (ulimit) on the number of file descriptors a single process can hold (often 1024 or 4096 by default). 

**Q2:** "How do you pipe the output of one process into another? Explain standard streams."
*Expected:* `stdout` (fd 1) of process A is connected to `stdin` (fd 0) of process B via an OS-level pipe buffer. In Node.js, this is `child_process.spawn().stdout.pipe(...)`.

---

## Computer Networks — WebSockets (CodeSync AI)

**Q:** "Why WebSockets for CodeSync AI instead of Server-Sent Events (SSE) or long-polling?"
*Expected:* WebSockets provide full-duplex, bidirectional communication. SSE is strictly unidirectional (server to client). Collaborative editing requires the client to push keystrokes (upstream) and receive others' keystrokes (downstream) with the lowest possible latency.

---

## Databases / Consistency — Operational Transformation

**Q:** "CodeSync AI lets multiple users edit the same file. Two users on opposite sides of the world insert a character at index 5 at the exact same millisecond. How do you resolve this?"
*Expected:* Acknowledge that basic WebSockets will result in desync. Mention Operational Transformation (OT) or CRDTs (Conflict-free Replicated Data Types). OT requires a central server to sequence and transform the operations (like Google Docs). CRDTs are mathematically commutative and don't strictly require a central sequencer (like Figma/Zed).


---

## Exhaustive CS Fundamentals (Theoretical & SQL)
*These are additional deep-dive topics specifically assigned to this interview loop to ensure 100% breadth coverage across all 20 interviews.*

### OS - Memory Management
**Paging vs Segmentation:** What is the difference? How do they solve External and Internal Fragmentation?

### CN - Protocols
**TCP Handshake:** Explain the 3-way handshake (`SYN`, `SYN-ACK`, `ACK`) and the 4-way teardown.

### SQL - Query 1
**Nth Highest Salary:** Write a query to find the 3rd highest salary without using `LIMIT`/`OFFSET`, using standard window functions (`DENSE_RANK()`).



---

## Master Question Bank — Assigned Slice (Round 11)
*These questions are your assigned coverage from the 275-question Master Bank. Every question appears exactly once across all 20 interviews.*

### Operating Systems (OS)
- Dining philosophers problem
- Readers-writers problem

### Database Management Systems (DBMS)
- Stored procedure — pros/cons vs application-layer logic
- Database sharding — common strategies

### SQL — Practical Query Problems
- INNER JOIN vs WHERE-clause filtering of joined tables

### Computer Networks (CN)
- Socket vs port
- TCP congestion control (slow start, congestion avoidance)

### Object-Oriented Programming (OOP)
- Encapsulation and access modifiers

### Linux
- What is a daemon process?

### Security
- Man-in-the-middle attack — how HTTPS prevents it

### Git
- .gitignore — behavior on already-tracked files

### Language Internals — Java
- Abstract class vs interface in Java (default methods, multiple inheritance)

### Language Internals — C++
- Pass-by-value vs pass-by-reference vs pass-by-pointer

