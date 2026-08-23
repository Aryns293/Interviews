# 🧠 The Interview Loop — Complete Breakdown

A practical, no-fluff guide to what actually happens in a modern SWE interview loop. Every round, every topic, and what the interviewers are **actually** filtering for.

---

## The Loop at a Glance

| # | Round | Duration | Core Focus | Pass Bar |
| :---: | :--- | :---: | :--- | :--- |
| 0 | Online Assessment *(pre-req)* | 60–90 min | DSA + MCQs | Auto-filtered by score |
| 1 | Pure DSA | 60 min | Algorithmic depth | Correct + optimal + handles follow-ups |
| 2 | CS Fundamentals | 60–75 min | OS / DBMS / CN / SQL / Linux / OOPS / Internals | Depth under follow-up chains |
| 3 | Past Experience & Project Grilling | 45–60 min | Internships, ownership, tech choices, debugging | Can you defend your own work 3 levels deep |
| 4 | Applied Coding: Backend + Full-Stack | 60–90 min | APIs, DB design, React, system wiring | Can you build something real end-to-end |
| 5 | LLD & Light System Design | 60–75 min | Design thinking, patterns, scale basics | Can you design, not just recall |
| 6 | Behavioral (STAR) | 45 min | Culture fit, teamwork, conflict resolution | Self-awareness, structured communication |
| 7 | Hiring Manager / Bar Raiser | 45–60 min | Judgment, leadership principles, expectations | Ownership, maturity, "hire/no-hire" decider |

> **Note:** This represents a highly comprehensive 100% coverage loop. Realistically, companies will mix and match these rounds, but if you prepare for all 7 stages, you are covered for virtually any SWE interview.

---

## Round 0 — Online Assessment
*(Gatekeeper — you never see a human if you don't clear this)*

- 2–3 DSA problems on HackerRank / HackerEarth / Codility, strict timer
- Often bundled with **15–20 MCQs** on OS, DBMS, OOPS, aptitude, and output-prediction (C/Java code snippets)
- **The trap:** untimed practice doesn't train panic-management. Always practice under a live countdown. The OA tests whether you can execute under pressure, not just whether you know the algorithm.

**Common MCQ traps:**
- Output prediction of pointer arithmetic, bitwise ops, static initialization order
- Process scheduling numerical (FCFS, SJF, Priority, Round Robin)
- Normalization MCQs (identify the normal form of a given schema)
- Find the bug in a Java/C++ snippet

---

## Round 1 — Pure DSA (Medium + Hard)

| Topic Area | What Gets Asked |
| :--- | :--- |
| Arrays / Strings | Sliding window, two-pointer, prefix sum, anagram/substring problems |
| Trees / Graphs | DFS/BFS, topological sort, LCA, cycle detection, tree serialization |
| Dynamic Programming | 1D/2D DP, knapsack variants, string DP (LCS, edit distance), matrix chain |
| Linked List / Heap / Stack | LRU Cache, Merge K Sorted Lists, monotonic stack, next greater element |
| Backtracking | N-Queens, subsets/permutations, sudoku solver, word search |
| Greedy + Intervals | Activity selection, meeting rooms, jump game, interval merge |
| Bit Manipulation | XOR tricks, counting set bits, power of 2, single number variants |

**Format:** 2 problems, medium → hard, live coding on a shared editor (no IDE autocomplete, no Stack Overflow).

**What's actually scored:**
1. Clarifying questions asked *before* coding
2. Brute force stated *before* optimizing (never skip this — interviewers mark it)
3. Dry-run on edge cases without being prompted (empty input, single element, duplicates)
4. **#1 differentiator:** How you react when the interviewer changes a constraint mid-solve — this is where candidates are separated

---

## Round 2 — CS Fundamentals

> This is the round that filters people who only grinded LeetCode.

Opens with one easy-medium coding warm-up (~15 min), then goes **3–4 follow-ups deep** on 4–5 of the topics below based on your resume/stack.

| Topic | What Gets Asked |
| :--- | :--- |
| **OS** | Process vs thread, deadlock (4 conditions + Banker's Algorithm live), paging/segmentation, thrashing, Belady's Anomaly, CPU scheduling (Gantt chart numericals: FCFS/SJF/SRTF/RR/Priority), context switching cost, mutex vs semaphore, critical section |
| **DBMS** | ACID properties (with examples of each breaking), live normalization to 3NF/BCNF, B+ Tree indexing rationale, isolation levels table (dirty read / non-repeatable read / phantom read), 2PL & deadlocks, CAP theorem basics |
| **SQL** | Live query writing: Nth highest salary, self-joins, window functions (`RANK` / `DENSE_RANK` / `ROW_NUMBER` / `LAG` / `LEAD`), `WHERE` vs `HAVING`, subqueries vs CTEs, `EXPLAIN` plan basics |
| **Computer Networks** | "What happens when you type google.com?" (DNS → TCP 3-way handshake → TLS → HTTP/2), TCP vs UDP (when to use each), HTTP status codes (401 vs 403, 301 vs 302, 429, 503), REST idempotency (is POST idempotent? is PUT?), cookies vs sessions vs JWT, WebSockets vs HTTP |
| **Linux** | `chmod` octal notation, hard vs soft links, `kill -9` vs `SIGTERM`, `find` / `grep` / `lsof` / `netstat` usage, what a pipe does at OS level, `cron`, `top` / `htop` output reading |
| **OOPS** | 4 pillars with live code, abstract class vs interface (when each), diamond problem, virtual destructors, SOLID principles with violation examples in a code snippet, composition vs inheritance |
| **Language Internals** | Java: HashMap internals (hash, bucket, collision, treeification at 8 in Java 8+), JVM GC (minor/major GC, G1), checked vs unchecked exceptions. C++: stack vs heap, vtables, smart pointers (`unique_ptr` vs `shared_ptr`), RAII |
| **Git** | `merge` vs `rebase` (when each), conflict resolution walkthrough, `stash`, `cherry-pick`, `reflog`, `reset --hard` vs `--soft` |
| **Security Basics** | SQL injection + prepared statements, XSS vs CSRF (and mitigations), authentication vs authorization, why passwords are salted + bcrypt cost factor, HTTPS/TLS handshake |

---

## Round 3 — Past Experience & Project Grilling

> This round is where "technically fine but shallow on their own work" gets caught. Vague answers on your past internships or personal projects are the **single most common fresher red flag** reported by real interviewers.

| Segment | What Gets Asked |
| :--- | :--- |
| **Internship Deep-Dive** | "Walk me through the architecture of the feature you built during your internship." "What was the code review process like?" "How did your work impact the business metrics?" |
| **Tech Stack Justification** | "Why did you choose React/Node over X?" "Why MongoDB instead of Postgres for this specific feature?" You must defend your choices with real tradeoffs, not "because a tutorial used it." |
| **End-to-End Walkthrough** | "Trace a user registration from clicking 'Submit' to the data being saved in the database." |
| **Debugging & Challenges** | "What was the hardest bug you faced in this project? Walk me through how you isolated and fixed it." |
| **Scaling & Breaking Points** | "What breaks in this project if traffic increases 10x? 100x?" "How would you handle a sudden spike in concurrent writes?" |
| **Hindsight** | "If you were to rewrite this project today, what architecture or design patterns would you change?" |

---

## Round 4 — Applied Coding: Backend + Full-Stack

> Tests whether you can build something real — not just solve isolated algorithms. This combines backend endpoints, database wiring, and frontend integration into a single applied round.

| Segment | What Gets Asked |
| :--- | :--- |
| **REST API Design & Auth** | Design CRUD endpoints (`/v1/users`, etc.). Implement or explain JWT-based auth: token signing, refresh token rotation, middleware/guard pattern, protecting routes. |
| **Database Wiring & ORM** | Schema design, N+1 query problem (live debug), eager vs lazy loading, writing raw SQL for performance-critical paths, connection pooling. |
| **Core JavaScript & DOM** | Event loop + call stack (live trace), closures, `this` binding (`call`/`apply`/`bind`), event bubbling vs capturing, debounce vs throttle implementation. |
| **React Core & State** | `useState` / `useEffect` / `useMemo` / `useCallback` — when and why each, `useEffect` dependency array gotchas (stale closures), reconciliation. |
| **Performance & Caching** | React: preventing unnecessary re-renders (`React.memo`). Backend: Redis basics — cache-aside vs write-through, TTL, cache invalidation strategies. |
| **Async Patterns** | `Promise.all` vs `Promise.allSettled`, event loop + why Node.js is non-blocking, fetching data effectively (stale-while-revalidate). |

**Common live problems:**
- Build a debounced search bar with live results from a mock API
- Build a RESTful API for a to-do app with auth (Node/Express or Spring Boot)
- Debug a leaking endpoint that causes high DB load
- Fix a React component that re-renders on every keystroke

---

## Round 5 — LLD & Light System Design

| Segment | What Gets Asked |
| :--- | :--- |
| **LLD (Low-Level Design)** | One full design problem worked to actual classes: Parking Lot, Splitwise, Elevator System, Vending Machine, Tic-Tac-Toe, Library Management System. Expect to implement interfaces and maintain state. |
| **Design Patterns** | **Strategy** (Payment processors, sorting algorithms), **Observer** (Pub-sub, event listeners), **Factory** (Object creation abstraction), **Singleton** (DB connection pool), **State** (Elevator, vending machine), **Command** (Undo/redo logic). |
| **Thread-Safety** | "How would you make this class thread-safe?" "What happens if two users try to book the same ticket concurrently?" (Locks, atomic variables, concurrent data structures). |
| **Light System Design (HLD)** | Increasingly common even for freshers: "Design a URL Shortener," "Design a Rate Limiter," "Design a Notification Service" — they're checking basic scale reasoning (DB choice, caching layer, load balancer), not expecting senior HLD depth. |

---

## Round 6 — Behavioral (STAR)

> Tests teamwork, emotional maturity, and communication. A candidate who is technically brilliant but lacks empathy or teamwork skills gets filtered here.

| Segment | What Gets Asked |
| :--- | :--- |
| **Conflict Resolution** | "Tell me about a time you disagreed with a teammate or manager. How did you resolve it?" |
| **Failures & Growth** | "Tell me about a project that failed or a time you made a significant mistake. What did you learn?" |
| **Working Under Pressure** | "Describe a time you had to deliver under a very tight deadline. How did you prioritize?" |
| **Ambiguity** | "Tell me about a time you had to make a decision with incomplete information." |
| **Initiative** | "Give an example of a time you went above and beyond your assigned responsibilities." |

**The STAR Framework:**
Always answer with **S**ituation (context) → **T**ask (the challenge) → **A**ction (what *you* did) → **R**esult (quantifiable outcome).

---

## Round 7 — Hiring Manager / Bar Raiser

> The final decision-maker. This round evaluates your overall trajectory, leadership potential, and whether you raise the bar for the team.

| Segment | What Gets Asked |
| :--- | :--- |
| **Leadership Principles** | (Amazon specifically, but used everywhere). Every answer gets mapped to a core value (Ownership, Dive Deep, Disagree & Commit, Bias for Action, Customer Obsession). |
| **Resume Deep-Dive** | They'll pick one line off your resume at random and ask you to defend it 3 levels deep. If you claim a 20% performance increase, be prepared to explain exactly how you measured it. |
| **Judgment & Scenarios** | Open-ended judgment scenarios: "Your manager asks you to ship something you think is fundamentally broken — what do you do?" |
| **Expectations & Close** | Career goals, team preferences, why you want to work at this specific company, salary expectations (usually light-touch at the fresher level). |

---

## What's Missing from Most People's Prep

| Neglected Area | Why It Matters |
| :--- | :--- |
| **Constraint changes mid-solve** | The #1 DSA differentiator — practice with a friend who changes requirements after you start. |
| **Project depth** | If you can't walk through your own code 3 levels deep, it reads as copied work. |
| **SQL window functions** | Asked in ~60% of DBMS rounds but skipped in most tutorials. |
| **REST idempotency** | Deceptively simple concept — very commonly asked, very commonly fumbled. |
| **Thread-safety in LLD** | Almost every LLD problem gets a follow-up: "make it thread-safe." |
| **The `useEffect` closure trap** | React's most common follow-up after basic hooks. |
| **N+1 query problem** | Backend's most common "gotcha" — know it by name and know how to fix it. |
| **Behavioral prep** | Most people do 0 prep for Round 6/7 and lose the offer at the finish line. |

---

## Why This Structure Is Not Arbitrary

Each round tests a genuinely different failure mode:

- **Round 1** — filters *"can't code under pressure"*
- **Round 2** — filters *"crammed DSA, has no CS foundation"*
- **Round 3** — filters *"doesn't understand their own project / copied from a tutorial"*
- **Round 4** — filters *"knows syntax but can't build a real, working system"*
- **Round 5** — filters *"can code but can't design scalable, maintainable abstractions"*
- **Round 6** — filters *"lacks teamwork, empathy, or maturity"*
- **Round 7** — filters *"technically fine but poor judgment or poor culture fit"*

> A candidate can be excellent in 6 rounds and still get rejected by failing just one. "Well-rounded but not deep anywhere" fails far more often than "very strong in 5 areas, average in 2."
