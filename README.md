# 🧠 The Interview Loop — Complete Breakdown

A practical, no-fluff guide to what actually happens in a modern SWE interview loop. Every round, every topic, and what the interviewers are **actually** filtering for.

---

## The Loop at a Glance

| # | Round | Duration | Core Focus | Pass Bar |
| :---: | :--- | :---: | :--- | :--- |
| 0 | Online Assessment *(pre-req)* | 60–90 min | DSA + MCQs | Auto-filtered by score |
| 1 | Pure DSA | 60 min | Algorithmic depth | Correct + optimal + handles follow-ups |
| 2 | CS Fundamentals + Applied Coding | 60–75 min | OS / DBMS / CN / SQL / Linux / OOPS / Internals | Depth under follow-up chains |
| 3 | Backend + Full-Stack Applied Coding | 60–75 min | REST APIs, DB design, system wiring | Can you build something real end-to-end |
| 4 | Frontend + React (role-specific) | 45–60 min | Component design, state, performance, DOM | Clean, idiomatic, production-aware code |
| 5 | LLD + System Design + Project | 60–75 min | Design thinking + real ownership | Can you design, not just recall |
| 6 | Hiring Manager / Bar Raiser | 45–60 min | Behavioral + judgment + culture fit | Ownership, communication, self-awareness |

> **Note:** Not every company runs all 6 rounds. Rounds 3 and 4 are role-specific — backend roles skip Round 4, frontend/full-stack roles may get both. Rounds 0–2 and 5–6 are nearly universal.

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

## Round 2 — CS Fundamentals + Applied Coding

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

## Round 3 — Backend + Full-Stack Applied Coding

> Tests whether you can build something real — not just solve isolated algorithms.

This round is increasingly common and is often labeled "practical coding" or "take-home extension." You'll be given a partial system and asked to extend/fix/debug it.

| Segment | What Gets Asked |
| :--- | :--- |
| **REST API Design** | Design CRUD endpoints for a given entity (e.g., a blog, e-commerce cart). Follow REST conventions: correct HTTP verbs, status codes, route naming, versioning (`/v1/`), pagination (`?page=&limit=`) |
| **Authentication Flow** | Implement or explain JWT-based auth: token signing, refresh token rotation, middleware/guard pattern, role-based access control (RBAC), protecting routes |
| **Database Design** | Schema design for a given domain — normalize to 3NF, choose between SQL and NoSQL with justification, write migration scripts, handle soft deletes, design indexes for query patterns |
| **ORM & Query Optimization** | N+1 query problem (live debug), eager vs lazy loading, writing raw SQL for performance-critical paths, connection pooling |
| **Error Handling & Middleware** | Global error handler pattern, structured error responses (`{ error, message, statusCode }`), request validation (Joi/Zod), logging middleware (Morgan/Winston) |
| **Async Patterns** | Callback → Promise → async/await evolution, `Promise.all` vs `Promise.allSettled`, event loop + why Node.js is non-blocking, queue-based task offloading |
| **Message Queues / Background Jobs** | Why you'd use a queue (RabbitMQ/Kafka/BullMQ), producer-consumer model, retry logic, dead-letter queue concept |
| **Caching** | Redis basics — cache-aside vs write-through, TTL, cache invalidation strategies, when *not* to cache |
| **Testing** | Unit vs integration vs e2e, mocking external calls, testing async code, test pyramid |

**Common live problems:**
- Build a RESTful API for a to-do app with auth (Node/Express or Spring Boot)
- Debug a leaking endpoint that causes high DB load
- Add rate-limiting middleware to an existing Express app
- Write a migration to add a new indexed column without downtime

---

## Round 4 — Frontend + React (Role-Specific)

> Skipped for pure backend roles. Full-stack and frontend roles almost always include this.

| Segment | What Gets Asked |
| :--- | :--- |
| **Core JavaScript** | Event loop + call stack (live trace), closure, `this` binding (`call`/`apply`/`bind`), prototype chain, `var` vs `let` vs `const` (temporal dead zone), debounce vs throttle implementation, deep clone |
| **DOM & Browser APIs** | Event bubbling vs capturing (live demo), `stopPropagation` vs `preventDefault`, `localStorage` vs `sessionStorage` vs cookies, `fetch` vs `XMLHttpRequest`, `IntersectionObserver`, lazy loading images |
| **React Core** | Controlled vs uncontrolled components, `useState` / `useEffect` / `useRef` / `useMemo` / `useCallback` — when and why each, `useEffect` dependency array gotchas (stale closures), reconciliation + virtual DOM |
| **State Management** | Prop drilling → Context API → Redux/Zustand: when to escalate, Redux flow (action → reducer → store → selector), `useReducer` vs Redux for local vs global state |
| **Performance** | `React.memo`, `useMemo`, `useCallback` — what they actually prevent (re-renders), code splitting (`React.lazy` + `Suspense`), key prop importance in lists, avoiding unnecessary re-renders via profiler |
| **Component Design** | Design a reusable component live (Modal, Dropdown, Accordion, Tabs), prop interface design, compound component pattern, render props vs HOC vs hooks |
| **CSS & Styling** | Box model, `position` (relative/absolute/fixed/sticky), flexbox vs grid (when each), CSS specificity rules, BEM naming, CSS-in-JS tradeoffs (styled-components vs modules) |
| **Routing & Data Fetching** | React Router v6 basics, `loader` / `action` pattern, `useNavigate`, SWR/React Query — stale-while-revalidate, optimistic updates |
| **Accessibility (a11y)** | ARIA roles, `tabIndex`, focus management, semantic HTML, screen reader compatibility |

**Common live problems:**
- Build a debounced search bar with live results from a mock API
- Implement an infinite scroll list
- Design a multi-step form with validation
- Fix a React component that re-renders on every keystroke
- Implement a custom `useFetch` hook

---

## Round 5 — LLD + Light System Design + Project Deep-Dive

| Segment | What Gets Asked |
| :--- | :--- |
| **LLD** | One full design problem worked to actual classes: Parking Lot, Splitwise, Elevator System, Vending Machine, Tic-Tac-Toe, Library Management System — expect design pattern follow-ups (Strategy, State, Observer, Factory, Singleton) and thread-safety questions |
| **Light System Design** | Increasingly common even for freshers: "Design a URL Shortener," "Design a Rate Limiter," "Design a Notification Service" — they're checking basic scale reasoning (DB choice, caching layer, load balancer), not expecting senior HLD depth |
| **Project Deep-Dive** | Real cross-questioning: why this tech stack, end-to-end request walkthrough, hardest bug + how you actually debugged it, "what breaks at 10x traffic," what you'd redesign today, tradeoffs you made |

> This round is where "technically fine but shallow on their own work" gets caught. Vague answers on your own project are the **single most common fresher red flag** reported by real interviewers.

**LLD Design Patterns you must know cold:**
| Pattern | When asked |
| :--- | :--- |
| Strategy | Payment processor with multiple gateways, sorting algorithm switcher |
| Observer | Notification system, event bus, pub-sub |
| Factory / Abstract Factory | Object creation without specifying exact class |
| Singleton | DB connection pool, logger, config manager |
| State | Elevator, vending machine, order lifecycle |
| Decorator | Adding features to streams, UI components |
| Command | Undo/redo, task queue |

---

## Round 6 — Hiring Manager / Bar Raiser

| Segment | What Gets Asked |
| :--- | :--- |
| **Behavioral (STAR)** | Conflict with a teammate, a failure and what you learned, working under a tight deadline, disagreement with a decision, why this company/role — always answer with Situation → Task → Action → Result |
| **Leadership Principles (Amazon)** | Every answer gets mapped to an LP (Ownership, Dive Deep, Disagree & Commit, Bias for Action, Customer Obsession) — a technically flawless answer with no LP-shaped story can still fail this round |
| **Resume Walkthrough** | They'll pick one line off your resume at random and ask you to defend it 3 levels deep — don't list anything you can't explain cold |
| **Wildcard / Ambiguity** | Open-ended judgment scenario with no clean answer ("your manager asks you to ship something you think is broken — what do you do?") — they're testing reasoning and communication, not a "correct" answer |
| **Expectations / Close** | Salary band, location, notice period, team preferences — usually light-touch at fresher level |

**Prepare these STAR stories before the interview:**
1. A time you went above and beyond the scope of a task
2. A time you failed and what you changed afterwards
3. A conflict with a peer/teammate and how it was resolved
4. A time you had to make a decision with incomplete information
5. A time you took initiative without being asked

---

## What's Missing from Most People's Prep

| Neglected Area | Why It Matters |
| :--- | :--- |
| **Constraint changes mid-solve** | The #1 DSA differentiator — practice with a friend who changes requirements after you start |
| **Project depth** | If you can't walk through your own code 3 levels deep, it reads as copied work |
| **SQL window functions** | Asked in ~60% of DBMS rounds but skipped in most tutorials |
| **REST idempotency** | Deceptively simple concept — very commonly asked, very commonly fumbled |
| **Thread-safety in LLD** | Almost every LLD problem gets a follow-up: "make it thread-safe" |
| **The `useEffect` closure trap** | React's most common follow-up after basic hooks |
| **N+1 query problem** | Backend's most common "gotcha" — know it by name and fix |
| **Behavioral prep** | Most people do 0 prep for Round 6 and lose the offer here |

---

## Why This Structure Is Not Arbitrary

Each round tests a genuinely different failure mode:

- **Round 1** — filters *"can't code under pressure"*
- **Round 2** — filters *"crammed DSA, has no CS foundation"*
- **Round 3** — filters *"knows syntax but can't wire a real system"*
- **Round 4** — filters *"knows backend but no frontend intuition (or vice versa)"*
- **Round 5** — filters *"can code but can't design or doesn't understand their own project"*
- **Round 6** — filters *"technically fine but poor judgment / communication / culture fit"*

> A candidate can be excellent in 5 rounds and still get rejected by failing just one. "Well-rounded but not deep anywhere" fails far more often than "very strong in 4 areas, average in 2."
