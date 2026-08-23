# Project-Specific Interview Loops

This document contains 5 tailored interview loops based on your specific projects (Rolewize, QueueFlow, CodeSync AI, gitlight) and experience.

---

## INTERVIEW 1 — Backend/Infra-Heavy Product Company
*(Theme: your Rolewize internship + QueueFlow carry this whole loop)*

**Round 0 — Online Assessment**
* **DSA:** (1) Course Schedule II (topo sort), (2) Kth Largest Element in a Stream (heap)
* **MCQs:** FCFS/SRTF Gantt-chart numericals, 3NF vs BCNF identification, pointer arithmetic output prediction

**Round 1 — Pure DSA**
* **Medium:** Course Schedule (detect cycle, return valid build order) — direct DAG warm-up before Round 3 hits your gitlight DAG work.
* **Hard:** Alien Dictionary (topological sort from partial ordering).
* **Mid-solve twist:** "Now edges can be added dynamically, one at a time, and you must answer 'is it still valid?' after each insert." Forces you toward incremental cycle detection / union-find — this is the differentiator moment, don't just re-run full topo sort each time.
* **Scored on:** clarifying whether the graph is guaranteed connected, brute force stated first, edge cases (single node, self-loop, disconnected components).

**Round 2 — CS Fundamentals**
* **OS:** Banker's Algorithm live trace with 4 processes; explain why your BullMQ 3-attempt retry doesn't itself risk deadlock but a naive lock-based idempotency check could.
* **DBMS:** Live-normalize a denormalized "job + worker + queue" schema to 3NF. Explain B+Tree indexing rationale for a status column with low cardinality (index vs no index tradeoff).
* **SQL:** Write a query for "3rd highest-priority job per queue" using `DENSE_RANK()`. `WHERE` vs `HAVING` on a job-count-per-worker aggregate.
* **Redis internals:** RDB vs AOF persistence — which would you pick for QueueFlow and why? Explain what `BRPOP` blocking actually does at the socket level vs polling.
* **Security:** Walk through HMAC-SHA256 webhook signature verification step-by-step (this is literally what you built at Rolewize — expect them to ask you to whiteboard the exact algorithm, not just name-drop it).

**Round 3 — Past Experience & Project Grilling**
* "Walk me through your Rolewize webhook endpoint end-to-end — from the external service hitting your URL to the job landing in BullMQ."
* "You used dual-layer Redis/Postgres idempotency. Why both? What breaks if you only had Redis? What breaks if you only had Postgres?"
* "Your Redis cache uses SHA-256 hashing with 24-hour TTL to eliminate duplicate LLM calls. What's the cache key exactly? What happens if the same resume is uploaded but the LLM prompt/version changes — do you serve a stale result?"
* "QueueFlow: you claim O(1) enqueue/dequeue using LPUSH/BRPOP. Is that true after you add the 3-tier priority logic on top? Walk me through what actually happens across all 3 tiers for one dequeue call."
* "Why Redis over Kafka/RabbitMQ for a job queue that needs zero data loss? Defend that choice."
* "Hindsight: if you rebuilt QueueFlow today, what would you change?"

**Round 4 — Applied Coding: Backend + Full-Stack**
* **Live build:** Idempotent webhook receiver middleware (Express) — dedup using a Redis SET with TTL, return cached response on replay, without touching the DB on the hot path.
* **Live debug:** N+1 query problem in a Postgres+ORM job-listing endpoint fetching worker details per job — fix with eager loading.
* **Discussion:** Connection pooling — what happens to your Postgres pool under 1,000+ concurrent worker writes? What's your pool size strategy?
* **Live build:** Exponential backoff utility (2s/4s/8s) as a reusable function — implement it, then explain how you'd store retry state in Redis ZSET (your actual approach) vs in-memory (why the latter fails across worker restarts).

**Round 5 — LLD & Light System Design**
* **LLD:** Design Splitwise (classes for User, Expense, Split strategies, balance simplification). Follow-up: make balance settlement thread-safe when two settlements hit concurrently.
* **Pattern:** "Which pattern manages your Postgres connection pool?" (Singleton) → "Which pattern would let you swap retry strategies (fixed vs exponential vs fibonacci backoff) without touching worker code?" (Strategy)
* **Thread-safety (direct callback to your project):** "Two BullMQ workers pick up the same job simultaneously — how do you guarantee only one processes it? Walk through the exact locking mechanism, not just 'Redis handles it.'"
* **Light HLD:** "Design a Webhook Delivery System" — ingestion, signature verification, retry/backoff, dead-letter queue, at-least-once vs exactly-once semantics. (This is QueueFlow + Rolewize fused into one HLD problem — expect it.)

**Round 6 — Behavioral (STAR)**
* **Conflict:** "Tell me about a time during your Rolewize internship you disagreed with a code review comment or architectural decision."
* **Failure:** "What's a bug in QueueFlow or the internship codebase that took much longer to fix than expected? What did you learn?"
* **Pressure:** "You had a 2-month internship window to ship production webhook infrastructure. How did you prioritize?"
* **Ambiguity:** "Were the requirements for the idempotency layer fully spec'd, or did you have to make judgment calls? Walk through one."
* **Initiative:** "Tell me about mentoring 400+ students in C++ at RoundTable — what made someone actually improve vs plateau?"

**Round 7 — Hiring Manager / Bar Raiser**
* **Resume defense:** "You say your Redis caching eliminates duplicate LLM API calls. Eliminates, or reduces? What's your actual cache hit rate, and how did you measure it?"
* **Judgment:** "Your manager asks you to ship the webhook handler without the idempotency layer to hit a deadline. What do you do?"
* **Judgment:** "A partner's webhook starts retrying at 10x their agreed rate due to a bug on their end. Your dead-letter queue is filling up. What's your immediate move vs your next-day move?"
* **Close:** career goals, backend vs full-stack preference, expectations.

---

## INTERVIEW 2 — AI / Dev-Tools Startup
*(Theme: CodeSync AI carries this loop — real-time systems, sandboxing, AI integration)*

**Round 0 — Online Assessment**
* **DSA:** (1) Group Anagrams, (2) Word Break II (DP + backtracking)
* **MCQs:** HTTP status code identification (401 vs 403, 301 vs 302), event-loop output prediction, CSS specificity trap

**Round 1 — Pure DSA**
* **Medium:** Group Anagrams (hashing).
* **Hard:** Word Break II — return all valid segmentations (DP + backtracking, exponential blowup handling via memoization).
* **Mid-solve twist:** "Now the dictionary is streamed in and can grow while queries are being answered." Tests whether you default to re-computation or an incremental trie.
* **Scored on:** brute-force-first discipline, whether you catch the exponential explosion before being told, dry-run on empty string and no valid segmentation.

**Round 2 — CS Fundamentals**
* **CN:** Full "what happens when you type google.com" trace (DNS → TCP handshake → TLS → HTTP/2). Then: "Your Socket.IO connection — is that WebSocket the whole way, or does it fall back to HTTP long-polling? When?"
* **OOP:** 4 pillars with live code in JS-ish pseudocode; composition vs inheritance — "how did you model Docker-execution vs Judge0-fallback? Inheritance or composition, and why?"
* **JS/Node internals:** Event loop + call stack live trace with a `setTimeout` + `Promise.then` + synchronous code ordering puzzle. Explain why Node.js is non-blocking and how that let you handle multiple concurrent Socket.IO rooms on one process.
* **Security:** JWT structure (header.payload.signature), why you can't just trust the payload without verifying the signature, XSS risk in a code editor that renders user-submitted code as HTML.

**Round 3 — Past Experience & Project Grilling**
* "Two users type in the same line of the same file at the same second in CodeSync AI. What actually happens to the final text? Do you have any conflict resolution, or is it last-write-wins over Socket.IO?" (This is the gotcha — probe whether you've thought about OT/CRDT at all, since raw Socket.IO broadcast alone doesn't solve this.)
* "Your Docker sandbox disables networking, limits CPU/memory/processes, uses a non-root user, and enforces an execution timeout. Walk through what specific attack each of those mitigates. What happens if someone submits a fork bomb — does your process-limit actually stop it, or just slow it?"
* "When does execution fall back to Judge0? Is that decision made per-request or once at boot? What if Docker is healthy but just slow — do you have a timeout-based fallback, or only a hard-down check?"
* "Why Gemini specifically for code review, and why does the review need to know the room's selected language instead of detecting it from the code?"
* "Why MongoDB for user auth + persistent codebase storage instead of Postgres, given you used Postgres at Rolewize?"

**Round 4 — Applied Coding: Backend + Full-Stack**
* **Live build:** Debounced search bar hitting a mock API with live results, cancel in-flight stale requests.
* **Live debug:** Given a React component that re-renders on every keystroke of an unrelated input — find and fix the cause (likely missing memoization or a bad `useEffect` dependency array).
* **Live build:** JWT auth middleware with refresh-token rotation — access token short-lived, refresh token rotated and old one invalidated on use.
* **Discussion:** `Promise.all` vs `Promise.allSettled` — which would you use to kick off a Docker container spin-up and a Judge0 fallback call in parallel, and why?

**Round 5 — LLD & Light System Design**
* **LLD:** Design the core class model for a real-time collaborative code editor — Room, Session, User, Document, ChangeEvent. Where would operational transform or CRDT slot in if you added it later?
* **Pattern:** "Which pattern would you use to cleanly swap between the Docker executor and the Judge0 executor at runtime?" (Strategy) → "Which pattern fits your Socket.IO room broadcast — one room, many subscribed clients, all notified on change?" (Observer)
* **Thread-safety / concurrency:** "Two users hit 'Run Code' in the same room within the same second — do they share a container, or does each get their own? What's your isolation guarantee?"
* **Light HLD:** "Design the backend for CodeSync AI at 100x your current scale — multi-region rooms, thousands of concurrent editors." Push on: sticky sessions vs stateless with a shared pub/sub, sandboxing cost at scale, rate-limiting AI review calls.

**Round 6 — Behavioral (STAR)**
* **Conflict:** "Tell me about feedback you got on CodeSync AI's architecture (from a mentor, peer, or code review) that you initially disagreed with."
* **Failure:** "Was there a security or stability issue you discovered late in the Docker sandbox — something you missed on the first pass?"
* **Pressure:** "Flipkart GRiD 8.0 — what was the time crunch like, and how did you triage what to build vs skip?"
* **Ambiguity:** "How did you decide which 3 languages to support and which sandboxing tradeoffs to accept, with no one telling you the 'right' answer?"
* **Initiative:** "You built 3 substantial personal projects alongside an internship, a CGPA of 8.11, and a 1500+ problem CP grind. How did you actually decide where your time went?"

**Round 7 — Hiring Manager / Bar Raiser**
* **Resume defense:** "You list 'AI-powered code reviews... detecting bugs and generating precise code fixes.' How do you know the fixes are actually correct and not hallucinated? Do you validate them before showing the user?"
* **Judgment:** "PM wants to ship voice chat next sprint, but you're not confident it won't destabilize your real-time sync layer. What do you do?"
* **Judgment:** "A user reports their code disappeared during a collab session. You have no version history yet (per your own 'Future Improvements' list). What's your response — to the user, and to your own roadmap?"
* **Close:** why dev-tools/AI products specifically, expectations.

---

## INTERVIEW 3 — Systems / Low-Level Deep-Dive Company
*(Theme: gitlight carries this loop — hashing, compression, DAGs, OS internals)*

**Round 0 — Online Assessment**
* **DSA:** (1) Serialize and Deserialize Binary Tree, (2) Find Median from Data Stream (heap)
* **MCQs:** static initialization order, `find`/`grep`/`lsof` output prediction, process scheduling numerical (SRTF)

**Round 1 — Pure DSA**
* **Medium:** Serialize and Deserialize a Binary Tree — direct parallel to how you encoded Git tree objects into a binary format.
* **Hard (custom, built around your own project):** "Given a commit DAG where each commit can have multiple parents (merge commits), find the merge-base — the lowest common ancestor — of two commits." This is exactly what `git merge-base` does, and exactly what gitlight's DAG traversal sets you up for.
* **Mid-solve twist:** "Now there can be multiple valid lowest common ancestors (a diamond-shaped merge history) — return all of them, and explain what that means for a 3-way merge."
* **Scored on:** whether you recognize this maps to LCA-in-a-DAG (not a tree — no single parent, so classic binary-LCA tricks don't directly apply), clarifying questions about cycles (DAGs shouldn't have any — do you verify?).

**Round 2 — CS Fundamentals**
* **OS:** Paging vs segmentation, thrashing, and — tied to your project — "your object files are zlib-compressed and content-addressed. At what layer does the OS's own page cache interact with that, if at all?" Context-switch cost, mutex vs semaphore.
* **Hashing/Compression (direct project tie-in):** "Explain SHA-1 at a level where I believe you actually understand it, not just called a library. What's a hash collision, and why doesn't a SHA-1 collision in your object store immediately corrupt data?" "Why zlib specifically — what's the tradeoff between compression ratio and CPU time, and did you tune the compression level?"
* **Linux:** `chmod` octal notation, hard vs soft links (and how that differs from how Git stores blobs by content hash — no filename at the storage layer at all), `kill -9` vs `SIGTERM` and why a write-tree operation mid-flight matters for which one you'd use.
* **Git internals (theirs, not yours — testing if you understand real git beyond what you reimplemented):** merge vs rebase, reflog, cherry-pick, what `git gc` actually does to loose objects.

**Round 3 — Past Experience & Project Grilling**
* "Why rebuild Git's plumbing layer at all, when `isomorphic-git` or `libgit2` already exist? What did you actually learn that you couldn't have learned by reading Git's source?"
* "Walk me through, byte by byte, what happens from `gitlight add file.txt` to an object existing on disk." (header format → SHA-1 hash → zlib compress → path `.git/objects/<2>/<38>`.)
* "You claim objects are 'fully compatible with real Git.' How did you actually verify that — did you diff your generated objects against `git cat-file` output on the same content, or is that claim untested?"
* "Your diff uses LCS, same as real Git. What's the time/space complexity of your LCS implementation, and does it degrade badly on large files? Real Git has heuristics (like the 'xdiff' algorithm) for exactly this — did you hit that wall?"
* "You used the Command Pattern for each Git command. Why that pattern specifically — what would break if you'd just used a switch statement instead?"

**Round 4 — Applied Coding: Backend + Full-Stack**
* **Live build:** Implement a minimal content-addressable store — hash arbitrary content with SHA-1 (or SHA-256), store it at a path derived from the hash, retrieve by hash. (Directly reproduces the core of what you built — expect them to ask you to do it live, no notes.)
* **Live build:** Rate-limiting middleware (token bucket or sliding window) for an Express API.
* **Discussion:** Event loop trace — a script mixing `setTimeout(fn, 0)`, a `Promise.resolve().then()`, and a synchronous `for` loop; predict exact output order.
* **Live debug:** N+1 query problem in a generic endpoint.

**Round 5 — LLD & Light System Design**
* **LLD (direct project tie-in):** Design the class hierarchy for Git's object model — Blob, Tree, Commit. Composition or inheritance? Where does content-addressing fit as a cross-cutting concern (a separate ObjectStore class, or baked into each object)?
* **LLD (classic, second problem):** Design an Elevator System — work it to the State pattern (Idle/MovingUp/MovingDown/DoorOpen states).
* **Pattern:** Defend your actual Command Pattern choice from gitlight in more depth — how would undo work if you extended each command with an `unexecute()` method?
* **Light HLD:** "Design a Rate Limiter" (token bucket vs sliding window log vs sliding window counter — tradeoffs, and where would you store counters: in-memory, Redis, or a dedicated service, given you've already used Redis primitives before).

**Round 6 — Behavioral (STAR)**
* **Conflict:** "Tell me about a technical disagreement with a mentor or senior engineer at Rolewize about an architecture choice."
* **Failure:** "You made the SIH 2025 internal round but not further — walk me through what you'd do differently next time."
* **Pressure:** "How did you keep an A+ in OS/DBMS/CN while running the CP grind and building 3 side projects? What gave first when things got tight?"
* **Ambiguity:** "gitlight had no spec — no one told you which Git commands to implement or in what order. How did you decide?"
* **Initiative:** "Nobody assigned you 'go reimplement Git.' Why did you actually build it?"

**Round 7 — Hiring Manager / Bar Raiser**
* **Resume defense:** pick a random project bullet and demand justification — "Solved 1,500+ DSA and CP problems" — "roughly what fraction were medium/hard vs easy, and what's a problem that actually changed how you think, not just one you solved?"
* **Judgment (verbatim classic bar-raiser prompt, deliberately harder because you understand internals):** "Your manager asks you to ship something you believe is fundamentally broken — a merge algorithm you know produces wrong results on 5% of inputs, but the deadline is tomorrow. What do you do?"
* **Judgment:** "You find a subtle correctness issue in a system your whole team relies on (like a hashing bug that only shows up in cross-platform newline handling). It's not on fire yet. Do you raise it immediately or fix it quietly?"
* **Close:** interest in systems/infra roles specifically vs general full-stack, expectations.

---

## INTERVIEW 4 — General Product-Based Company (Balanced Loop)
*(Theme: touches all three projects evenly — this is the "any mid-size SaaS" version)*

**Round 0 — Online Assessment**
* **DSA:** (1) Top K Frequent Elements, (2) Median of Two Sorted Arrays
* **MCQs:** normal-form identification, Java/C++ snippet bug-finding, HTTP status codes

**Round 1 — Pure DSA**
* **Medium:** Top K Frequent Elements (heap or bucket sort).
* **Hard:** Median of Two Sorted Arrays (binary search on the smaller array, `O(log(min(m,n)))` — expect them to reject an `O(m+n)` merge as "not optimal enough" and push for the log-time solution).
* **Mid-solve twist:** "Now the two arrays are actually two live streams that keep growing." Forces a rethink from a one-shot binary search to a two-heap running-median structure.
* **Scored on:** whether you state the naive merge approach first (never skip this), then push to the optimal solution unprompted or under a nudge.

**Round 2 — CS Fundamentals**
* **OS:** Deadlock's 4 conditions with a concrete example from your own systems (e.g., could your dual-layer Redis/Postgres idempotency ever deadlock? Walk through why or why not).
* **DBMS:** ACID properties, each with a real example of it breaking; isolation levels table (dirty read / non-repeatable read / phantom read) with a scenario for each.
* **CN:** "What happens when you type google.com" full trace; REST idempotency — "Is POST idempotent? Is PUT? Where does your webhook retry logic from Rolewize depend on this distinction?"
* **OOP:** SOLID principles — given a code snippet violating Single Responsibility, identify and fix it.
* **Git (tooling, not gitlight internals):** merge vs rebase — when would you use each on a feature branch with 3 collaborators?
* **Security:** SQL injection + prepared statements, why passwords are salted, bcrypt cost factor.

**Round 3 — Past Experience & Project Grilling**
* One end-to-end walkthrough per project, kept tight: "Trace CodeSync AI from a user hitting 'Run Code' to output appearing." "Trace QueueFlow from a job being enqueued to it completing or dead-lettering." "Trace gitlight from add to commit."
* "Why Redis over Kafka/RabbitMQ for QueueFlow? Why MongoDB over Postgres for CodeSync AI, when you clearly know Postgres well from Rolewize? Defend both choices with real tradeoffs — not 'a tutorial used it.'"
* "What breaks in QueueFlow if traffic goes from 1,000 concurrent jobs to 100,000? Where's the first bottleneck — Redis, Postgres, or your worker count?"
* **Hindsight:** "If you rewrote any one of your three projects today, which one and what would change first?"

**Round 4 — Applied Coding: Backend + Full-Stack**
* **Live build:** REST CRUD API for a to-do app with JWT auth middleware protecting routes (Node/Express).
* **Live build:** Cache-aside pattern with Redis — read-through on miss, TTL-based invalidation, for a "get job status" endpoint.
* **Live debug:** An endpoint leaking DB connections under load — find the missing `.release()`/connection-pool bug.
* **Discussion:** `useMemo`/`useCallback` — when do they actually help vs add overhead for no reason.

**Round 5 — LLD & Light System Design**
* **LLD:** Design a Library Management System — Book, Member, Loan, Reservation, fine calculation.
* **Pattern:** Factory (object creation abstraction for different job types in a queue), Singleton (DB connection pool).
* **Thread-safety:** "Two members try to reserve the last copy of a book at the same time — how do you guarantee only one succeeds?"
* **Light HLD:** "Design a URL Shortener" — hash-based vs counter-based ID generation, read-heavy caching strategy, collision handling.

**Round 6 — Behavioral (STAR)**
* Standard STAR set (conflict, failure, pressure, ambiguity, initiative) — but every answer gets immediately followed with "map that back to something specific — Rolewize, one of your 3 projects, SIH, or GRiD. Generic answers get pushed back on here."

**Round 7 — Hiring Manager / Bar Raiser**
* **Resume defense:** interviewer picks one line at random (could be CGPA, GRiD selection, mentoring 400+ students, or any project bullet) — must be defended 3 levels deep, live.
* **Judgment:** "You're asked to ship a feature you believe is fundamentally broken. What do you do?" (the doc's canonical prompt)
* **Close:** career goals, team preferences (backend/full-stack/infra), why this company, salary expectations (light-touch at fresher level).

---

## INTERVIEW 5 — FAANG-Style / Amazon-Style Bar Raiser Loop
*(Theme: hardest DSA, hardest resume grilling, stacked judgment scenarios — this is the loop that actually tests whether you're "well-rounded but not deep anywhere" or genuinely strong)*

**Round 0 — Online Assessment**
* **DSA:** (1) Longest Increasing Path in a Matrix (DFS + memo), (2) Word Search II (Trie + backtracking)
* **MCQs:** bitwise-op output prediction, priority scheduling numerical, aggressive time pressure (this OA is timed tighter than the others)

**Round 1 — Pure DSA**
* **Medium-Hard:** Longest Increasing Path in a Matrix.
* **Hard:** Word Search II — build a Trie from the word list, then backtrack through the grid, pruning dead branches early.
* **Mid-solve twist:** "The word list is now 100,000 words instead of 10 — your current approach just timed out. Optimize." (Forces you to talk about Trie-pruning aggressively, removing found words/leaves from the Trie as you go.)
* **Scored harder here than the other 4 interviews:** brute force must be stated in under 60 seconds, and the interviewer will introduce the constraint change without warning, mid-line, to see if you flinch.

**Round 2 — CS Fundamentals**
*(Rapid-fire, 3-4 follow-ups deep on each, moving fast, no time to think out loud slowly)*

* **OS:** Banker's Algorithm live numerical + "does your BullMQ retry logic have any theoretical deadlock risk? Prove it either way."
* **DBMS:** 2PL and deadlocks, CAP theorem — "QueueFlow uses Redis (AP-leaning) and Postgres (CP-leaning) together. What consistency model does the system as a whole actually give you?"
* **CN:** TCP vs UDP — "Socket.IO falls back through several transports. Which one is closest to UDP in behavior, and why doesn't Socket.IO just use raw UDP for a code editor?"
* **Language internals:** HashMap internals (hashing, bucket, collision, treeification) — even though your stack is JS/Node, expect at least one Java-flavored internals question since it's on your resume as a supported execution language.
* **Security:** HMAC-SHA256 walkthrough again, but faster, and followed immediately by "now break it — how would you attack a webhook endpoint that verifies HMAC-SHA256 but has a timing-unsafe string comparison?"

**Round 3 — Past Experience & Project Grilling**
*(All three projects + the internship, in one sitting, defending every specific number)*

* **"Zero data loss" (QueueFlow):** "Is that literally true? What's the exact window between a job being `LPUSH`'d to Redis and durably persisted to Postgres? If the process crashes in that window, what happens to the job?"
* **"100% fault tolerance against worker node failures":** "100%? What if the failure is a network partition where the worker is alive but unreachable — does your system distinguish 'dead' from 'partitioned'?"
* **"3-attempt retry" (BullMQ, Rolewize):** "What happens on attempt 4? Silent drop, dead-letter, or alert? If you don't remember exactly, say so — don't guess."
* **"Eliminating duplicate LLM API calls" via Redis+SHA-256:** "Eliminating, or reducing? Give me the actual mechanism that could still let a duplicate through."
* **"Fully compatible with real Git" (gitlight):** "Compatible how — did you test it, or is that an aspirational claim?"

**Round 4 — Applied Coding: Backend + Full-Stack**
* **Live build:** A distributed lock using Redis (`SET key val NX PX ttl`) — implement acquire/release correctly, including the classic bug of releasing a lock you no longer own (use a unique token + compare-before-delete).
* **Live build:** Generic retry-with-exponential-backoff-and-jitter utility function.
* **Live debug:** A React hook with a stale-closure bug (state read inside a `useEffect` that's one render behind) — find it fast, under time pressure.
* **Live debug:** N+1 query fix, but with a follow-up: "now do it without changing the ORM call — using a raw SQL join instead."

**Round 5 — LLD & Light System Design**
* **LLD:** Parking Lot, full depth — multi-floor, multiple vehicle types, a payment Strategy, spot-assignment algorithm. Then: "Make spot assignment thread-safe under 50 concurrent entry requests for the last 3 spots."
* **Patterns, rapid-fire:** Strategy, Observer, Factory, Singleton, State, Command — one real example from your own projects for each, on the spot, no prep time.
* **Light HLD (direct scale-up of your own project):** "Design QueueFlow, but it needs to handle 1,000,000 jobs/day across multiple regions with sub-second p99 latency. What changes — sharding strategy for the queue, cross-region consistency, how priority tiers survive a regional failover?"

**Round 6 — Behavioral (STAR)**
*(Faster pace, less hand-holding, more "give me a different example" pushback if the first answer feels rehearsed)*

* "Give me three examples, 90 seconds each: a disagreement, a failure, and a time you had incomplete information." (Tests structured communication under real time pressure, not just STAR knowledge.)
* Deep-dive on one (the Rolewize idempotency design): "was there a moment you had to make a call with no clear right answer? What did you decide, and would you decide the same way again?"
* "You've solved 1,500+ problems and hold Knight/Specialist ratings. Tell me about a contest or problem where you completely misjudged your approach and had to scrap it mid-contest. What did that teach you about your own blind spots?"

**Round 7 — Hiring Manager / Bar Raiser**
* **Full resume grilling, stacked:** pick two lines (not one), defend both 3 levels deep, back to back, no break between.
* **Judgment, stacked with a follow-up twist:** "Your manager asks you to ship something you believe is fundamentally broken. What do you do?" → (after your answer) "Now your manager says 'I hear you, but we're shipping it anyway — it's my call.' What do you do now?" (Ownership vs Disagree & Commit, explicitly testing both LPs in sequence.)
* "Why should we hire you over another candidate with a similar LeetCode rating and similar projects? Be specific — not 'I work hard.'"
* **Close:** expectations, but with a harder edge — "what would make you turn down an offer from us?"

---

## What Actually Needs the Most Prep (Given Your Profile)

1. **The concurrency/conflict-resolution gap in CodeSync AI** (Round 3, Interview 2) will get hit hard — you don't currently have OT/CRDT in the project, and "last-write-wins over Socket.IO" is a real weak point if pushed. Have an honest, thought-through answer ready, even if the honest answer is "I know this is a gap, here's how I'd close it."
2. **Defending absolute claims** ("zero data loss," "100% fault tolerance," "eliminating duplicate calls") — soften these to precise, defensible language before the interview, or be ready to defend them exactly as written under real pressure (Interview 5, Round 3/7 hits this the hardest).
3. **SQL window functions** — asked in ~60% of real DBMS rounds per the source doc; make sure `RANK` / `DENSE_RANK` / `LAG` / `LEAD` are fluent, not just recognized.
4. **Thread-safety follow-ups on LLD** — almost guaranteed on every LLD round; you have real material to draw from (BullMQ double-processing, Redis-based locks), so lean on your own systems rather than generic textbook answers.
