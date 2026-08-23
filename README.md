# The Loop at a Glance

| Round | Duration | Core Focus | Pass Bar |
| :--- | :--- | :--- | :--- |
| 0 | Online Assessment (pre-req, not one of the 4) | 60–90 min | DSA + MCQs | Auto-filtered by score |
| 1 | Pure DSA | 60 min | Algorithmic depth | Correct + optimal + handles follow-ups |
| 2 | CS Fundamentals + Applied Coding | 60–75 min | OS/DBMS/CN/SQL/Linux/OOPS/Internals | Depth under follow-up chains |
| 3 | LLD + System Design + Project | 60–75 min | Design thinking + real ownership | Can you design, not just recall |
| 4 | Hiring Manager / Bar Raiser | 45–60 min | Behavioral + judgment + culture fit | Ownership, communication, self-awareness |

## Round 0 — Online Assessment
*(gatekeeper, mention because it's non-negotiable)*

* 2–3 DSA problems (HackerRank/HackerEarth/Codility), strict timer
* Often bundled with 15–20 MCQs on OS, DBMS, OOPS, aptitude, output-prediction (C/Java code snippets)
* You never see a human if you don't clear this. Practice under a live countdown timer specifically — untimed practice doesn't train the panic-management this round requires.

## Round 1 — Pure DSA (Medium + Hard)

| Topic Area | What gets asked |
| :--- | :--- |
| Arrays / Strings | Sliding window, two-pointer, prefix sum — medium tier |
| Trees / Graphs | DFS/BFS, topological sort, LCA, tree serialization |
| Dynamic Programming | 1D/2D DP, knapsack variants, string DP (edit distance) |
| Linked List / Heap / Stack | Design-style hards (LRU Cache, Merge K Sorted Lists) |
| Backtracking | N-Queens, subsets/permutations, word search |

**Format:** 2 problems, medium → hard, live coding on a shared editor, no IDE autocomplete.  
**What's actually scored:** clarifying questions asked upfront, brute force stated before optimizing, dry-run on edge cases without being prompted, and — most heavily — how you respond when the interviewer changes a constraint mid-solve (this is the #1 differentiator, covered in detail in my earlier message).

## Round 2 — CS Fundamentals + Applied Coding

This is the round that filters people who only grinded LeetCode. Real companies almost always open with one easy-medium coding problem to "warm up," then spend the rest going deep on fundamentals.

| Topic | What gets asked (from earlier + new) |
| :--- | :--- |
| OS | Process vs thread, deadlock (4 conditions + Banker's Algo live problem), paging/segmentation, thrashing, Belady's Anomaly, CPU scheduling (Gantt chart numericals), context switching, mutex vs semaphore |
| DBMS | ACID, live normalization to 3NF/BCNF, B+ Tree indexing rationale, isolation levels table, 2PL & deadlocks |
| SQL | Live query writing: Nth highest salary, self-joins, window functions (RANK/DENSE_RANK/ROW_NUMBER), WHERE vs HAVING |
| Computer Networks | "What happens when you type google.com and hit Enter" (DNS → TCP 3-way handshake → TLS → HTTP), TCP vs UDP, HTTP status codes (401 vs 403, 301 vs 302), REST idempotency (is POST idempotent? is PUT?), cookies/sessions/JWT |
| Linux | `chmod` octal notation, hard vs soft links, `kill -9` vs `SIGTERM`, `find`/`grep`/`lsof` usage, what a pipe does at OS level |
| OOPS | 4 pillars with live code, abstract class vs interface, diamond problem, virtual destructors, SOLID violations shown in a code snippet |
| Language Internals | HashMap internals (hashing, collision handling, treeification in Java 8+), JVM/GC basics, checked vs unchecked exceptions — or for C++: stack vs heap, vtables, smart pointers |
| Git | merge vs rebase, conflict resolution walkthrough, stash, cherry-pick |
| Security basics | SQL injection + prepared statements, XSS vs CSRF, authentication vs authorization, why passwords are salted |

**Realistic time split:** ~15 min coding warm-up, then rapid-fire across 4–5 of the above topics chosen based on your resume/stack — they won't cover all of it, but they'll go 3–4 follow-ups deep on whichever ones they pick.

## Round 3 — LLD + Light System Design + Project Deep-Dive

| Segment | What gets asked |
| :--- | :--- |
| LLD | One full design problem worked to actual classes: Parking Lot, Splitwise, Elevator System, Vending Machine, Tic-Tac-Toe — expect design pattern follow-ups (Strategy, State, Observer) and thread-safety questions |
| Light System Design | Increasingly common even for freshers: "Design a URL Shortener" or "Design a Rate Limiter" — they're checking basic scale reasoning (DB choice, caching, load), not expecting HLD-senior depth |
| Project Deep-Dive | Real cross-questioning: why this tech stack, end-to-end request walkthrough, hardest bug + how you actually debugged it, "what breaks at 10x traffic," what you'd redesign today |

This round is where "technically fine but shallow on their own work" gets caught — vague answers on your own project are the single most common fresher red flag reported by real interviewers.

## Round 4 — Hiring Manager / Bar Raiser

| Segment | What gets asked |
| :--- | :--- |
| Behavioral (STAR) | Conflict with a teammate, a failure and what you learned, working under a tight deadline, disagreement with a decision, why this company/role |
| Leadership Principles (Amazon specifically) | Every answer gets mapped to an LP (Ownership, Dive Deep, Disagree & Commit, Bias for Action) — a technically flawless answer with no LP-shaped story can still fail this round at Amazon |
| Resume walkthrough | They'll pick one line off your resume at random and ask you to defend it in depth — don't put anything you can't go 3 levels deep on |
| Wildcard/ambiguity question | Sometimes an open-ended judgment scenario with no clean answer ("your manager asks you to ship something you think is broken — what do you do?") — they're testing reasoning and communication, not a "correct" answer |
| Expectations/negotiation close | Salary band, location, notice period — usually light-touch at fresher level |

## Why this 4-round structure is "correct" and not arbitrary

Each round tests a genuinely different failure mode, and real companies design it this way on purpose:

* **Round 1** filters "can't code under pressure"
* **Round 2** filters "crammed DSA, has no CS foundation"
* **Round 3** filters "can code but can't design or doesn't actually understand their own project"
* **Round 4** filters "technically fine but poor judgment/communication/culture fit"

A candidate can be excellent in 3 rounds and still get rejected by failing just one — this is why "well-rounded but not deep anywhere" fails far more often than "very strong in 3 areas, average in 1."
