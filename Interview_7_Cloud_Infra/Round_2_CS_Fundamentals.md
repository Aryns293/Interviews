# Round 2 — CS Fundamentals
**Interview:** Cloud Infrastructure / Platform Engineering Company
**Duration:** 60–75 minutes
**Theme:** OS internals, Linux, containers, networking. Every topic maps directly to your CodeSync AI Docker sandbox.

---

## OS — Virtual Memory, Paging, and cgroups

**Q1:** Explain virtual memory. What does the OS do when a process accesses a page that's not in physical RAM?

*Expected:* Page fault → OS checks page table → if the page is on disk (swapped), it reads it into RAM (page-in). If RAM is full, it picks a page to evict (LRU policy) and writes it to swap if dirty. The process resumes after the page-in.

**Q2 (direct project tie-in):**
> "Your CodeSync AI Docker sandbox enforces a memory limit of `X` MB on the container. What actually enforces that limit at the kernel level? Not Docker's flag — the underlying kernel mechanism."

*Expected:* **cgroups (Control Groups)** — a Linux kernel feature that limits, accounts for, and isolates resource usage (CPU, memory, network, disk I/O) for groups of processes. Docker sets a memory cgroup for each container. When the container tries to allocate memory beyond the limit, the kernel either:
1. Returns `ENOMEM` (OOM error) to the process
2. Triggers the OOM Killer (kills the most memory-hungry process in the cgroup)

**Q3:**
> "What's a Linux namespace, and how does it relate to Docker container isolation?"

*Expected:* Namespaces isolate what a process can see — PID namespace (container's PID 1 is not the host's PID 1), network namespace (container has its own virtual network interface), mount namespace (container has its own filesystem view), user namespace (root inside the container maps to an unprivileged user on the host — this is exactly why non-root containers are more secure).

---

## Linux — SIGTERM vs SIGKILL in a Container Context

**Q:**
> "In your Docker sandbox, when an execution timeout fires, do you send SIGTERM or SIGKILL to the container's process? What's the difference, and why does it matter?"

*Expected:*
- `SIGTERM` (15): a polite shutdown request. The process can catch it, clean up open files, flush buffers, write logs, and exit gracefully. Can be ignored by a process.
- `SIGKILL` (9): cannot be caught or ignored — the kernel terminates the process immediately. No cleanup.
- **For a code execution sandbox:** SIGKILL is usually correct — you don't trust the user's code to handle SIGTERM properly. Malicious code could catch SIGTERM and refuse to exit.
- **For a Node.js server:** SIGTERM is correct — you want graceful shutdown (finish in-flight requests, close DB connections).

---

## Computer Networks — Load Balancers and Health Checks

**Q1:** What's the difference between DNS round-robin and a proper load balancer?

*Expected:*
- DNS round-robin: different clients get different IPs from DNS — basic load distribution, but no health awareness. If one server dies, clients still get its IP until DNS TTL expires.
- Load balancer: actively health-checks backends, routes only to healthy ones, supports session affinity, SSL termination, connection pooling.

**Q2:**
> "Design a health-check endpoint for your CodeSync AI backend. What makes a good health check vs a fake 'always 200 OK' one?"

*Expected:*
- **Bad:** `GET /health → 200 OK` always. Tells the load balancer nothing — a broken server can still return 200 while its DB is down.
- **Good:**
  ```json
  GET /health → {
    "status": "ok",
    "checks": {
      "database": "ok",
      "redis": "ok",
      "docker_daemon": "ok"
    }
  }
  ```
  Return 503 if any dependency is unhealthy. The load balancer sees 503 and stops routing to that instance.
- **Separate readiness vs liveness:** Liveness = "is the process alive?" (restart if failing). Readiness = "is it ready to serve traffic?" (stop routing if failing, don't restart). Kubernetes separates these.

---

## Security — Non-Root Container and Privilege Escalation

**Q:**
> "You run your CodeSync AI sandbox as a non-root user inside Docker. Walk me through the specific attack that a root-inside-container user could launch that a non-root user cannot."

*Expected:*
- If a container escape vulnerability exists (e.g., a kernel exploit), a root process inside the container runs as root on the HOST if the container is running with `--privileged` or without user namespace remapping
- A non-root user inside the container (e.g., UID 1000) maps to UID 1000 on the host — limited privileges even after a container escape
- Additionally: root in a container can mount host filesystems, access host network interfaces, and write to `/proc` and `/sys` — dangerous even without a full escape
- Non-root containers cannot do any of the above


---

## Exhaustive CS Fundamentals (Theoretical & SQL)
*These are additional deep-dive topics specifically assigned to this interview loop to ensure 100% breadth coverage across all 20 interviews.*

### OS - Deadlocks
**4 Necessary Conditions:** Name and explain them (Mutual Exclusion, Hold and Wait, No Preemption, Circular Wait).

### DBMS - Indexing
**Composite Indexes:** If you have an index on `(A, B, C)`, will a query filtering by `WHERE B = 1 AND C = 2` use the index? (The concept of the Leftmost Prefix Rule).

### OOP - Patterns
**Factory:** What problem does the Factory Method solve compared to directly calling a constructor?

