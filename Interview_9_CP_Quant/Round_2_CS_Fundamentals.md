# Round 2 — CS Fundamentals
**Interview:** Competitive-Programming-Heavy / Quant-Adjacent Company
**Duration:** 45 minutes
**Style:** Fast-paced oral exam. I want precise answers, fast. If you don't know, say "I don't know" immediately so we can move to the next topic.

---

## Algorithms / Complexity Theory

**Q1:** "Memoization vs Tabulation in DP. When does Tabulation have a strictly worse space complexity than Memoization?"
*Expected:* When the state space is sparse. Tabulation initializes the entire N-dimensional array, taking O(N^k) space. Memoization only allocates memory for visited states.

**Q2:** "When does recursion depth actually matter?"
*Expected:* Stack overflow risk. In Python, recursion depth is ~1000 by default. In C++, it depends on stack size (typically 8MB). If your DP state requires 10^5 depth, recursion will crash; you must use tabulation or increase stack size manually.

---

## Operating Systems

**Q1 (Numerical, rapid fire):** 
"SRTF (Shortest Remaining Time First). P1(arrival 0, burst 8). P2(arrival 1, burst 4). P3(arrival 2, burst 2). Draw the timeline."
*Expected (in under 60 seconds):*
- T=0: P1 runs (remaining 8)
- T=1: P2 arrives (burst 4). P1 has 7 left. P2 preempts P1.
- T=2: P3 arrives (burst 2). P2 has 3 left. P3 preempts P2.
- T=2 to 4: P3 runs to completion.
- T=4 to 7: P2 resumes and finishes.
- T=7 to 14: P1 resumes and finishes.

---

## DBMS — Indexing Complexity

**Q1:** "You have a B+Tree index on an integer column. What is the time complexity of a range query `WHERE col BETWEEN X AND Y` returning K rows?"
*Expected:* O(log N + K). O(log N) to find the start node (X), and O(K) to traverse the linked leaves of the B+Tree to retrieve the K results.

**Q2:** "Why is that faster than a full table scan, which is O(N)?"
*Expected:* It's not just asymptotic time — it's disk I/O. A full table scan reads every disk page. A B+Tree index read reads a few index pages (often cached in RAM) and then only the required data pages. However, if K is very large (e.g., >20% of the table), the DB optimizer might choose a full table scan anyway because sequential disk reads are faster than millions of random index lookups.

---

## Math / Precision — Floating Point Traps

**Q1:** "Why does `0.1 + 0.2 !== 0.3` in JavaScript/Python/C++?"
*Expected:* IEEE 754 double-precision floats represent numbers in base-2 scientific notation. 0.1 and 0.2 are repeating fractions in binary (like 1/3 in base-10). They cannot be stored exactly. The small rounding errors add up.

**Q2:** "Where in a trading system or your own projects could floating-point precision silently cause a critical bug?"
*Expected:* Financial ledgers. If you use `float64` to store balances and subtract 0.1 repeatedly, you will eventually end up with `0.00000000000000004` instead of 0. Checking `if (balance == 0)` will fail. Fix: always use integers (store cents/paisa instead of dollars/rupees) or specialized `BigDecimal` classes.


---

## Exhaustive CS Fundamentals (Theoretical & SQL)
*These are additional deep-dive topics specifically assigned to this interview loop to ensure 100% breadth coverage across all 20 interviews.*

### OS - Deadlocks
**Detection & Recovery:** If a deadlock occurs, how does the OS detect it, and how does it recover (e.g., process termination, resource preemption)?

### CN - Models
**OSI vs TCP/IP Model:** Name the layers. At which layer do Routers operate? At which layer do Switches operate?

### OOP - Patterns
**Strategy:** How does it differ from a simple `switch` statement?



---

## Master Question Bank — Assigned Slice (Round 9)
*These questions are your assigned coverage from the 275-question Master Bank. Every question appears exactly once across all 20 interviews.*

### Operating Systems (OS)
- Gantt-chart numerical: compute waiting/turnaround time for FCFS/SJF/SRTF/RR/Priority given arrival & burst times
- Starvation — how is it solved (aging)?

### Database Management Systems (DBMS)
- Foreign key constraint and referential integrity
- JOIN types: INNER, LEFT, RIGHT, FULL OUTER, CROSS, SELF

### SQL — Practical Query Problems
- Correlated vs non-correlated subquery
- Employees earning more than their department's average

### Computer Networks (CN)
- Forward proxy vs reverse proxy
- Subnet mask and CIDR notation

### Object-Oriented Programming (OOP)
- Abstract method — can an all-abstract class just be an interface?
- Operator overloading

### Linux
- / (root) vs ~ (home)

### Security
- Refresh-token rotation — what it protects against

### Git
- git fetch vs git pull

### Language Internals — Java
- String vs StringBuilder vs StringBuffer

### Language Internals — C++
- malloc/free vs new/delete

