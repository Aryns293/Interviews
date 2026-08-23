# Round 2 — CS Fundamentals
**Interview:** General Product-Based Company
**Duration:** 60–75 minutes
**Style:** Balanced across OS, DBMS, CN, OOP, Git, Security. Tied to your resume throughout.

---

## OS — Deadlock + Your Systems

**Q1:** Name and explain the 4 necessary conditions for deadlock.

**Q2 (your project tie-in):**
> "In QueueFlow, you have dual-layer Redis/Postgres idempotency. Could this design ever deadlock? Walk me through why or why not using the 4 conditions."

*Expected:* Deadlock requires mutual exclusion + hold-and-wait + no preemption + circular wait. Redis SET operations are atomic and don't hold locks between operations. Postgres transactions hold row-level locks but QueueFlow's idempotency check is a single `INSERT OR IGNORE` — it doesn't hold a lock while waiting for Redis. So no circular wait → no deadlock. But if you had a design where you acquired the Redis lock THEN tried to acquire a Postgres row lock WHILE another transaction was doing the reverse — that would be the circular wait condition.

---

## DBMS — ACID + Isolation Levels

**Q1:** Explain each ACID property with a concrete example of what happens when it breaks.

| Property | Breaks when... |
|---|---|
| Atomicity | Payment deducts from A but crashes before crediting B |
| Consistency | A transfer takes balance below 0 (violating a constraint) |
| Isolation | Two concurrent transactions both read balance=100, both deduct 50, final balance=50 instead of 0 |
| Durability | A committed transaction is lost after a server crash (no WAL) |

**Q2:** What are the 3 read anomalies, and which isolation level prevents each?

| Anomaly | Description | Prevented by |
|---|---|---|
| Dirty Read | Reading uncommitted data from another transaction | Read Committed |
| Non-Repeatable Read | Same query returns different results within one transaction | Repeatable Read |
| Phantom Read | New rows appear matching a WHERE clause between two reads | Serializable |

---

## Computer Networks — REST + Your Webhook

**Q1:** Full "What happens when you type google.com" trace. You have 3 minutes.

**Q2:**
> "Is POST idempotent? Is PUT? And how does this distinction matter for your webhook retry logic at Rolewize?"

*Expected:*
- POST = NOT idempotent. Sending the same POST twice creates two resources.
- PUT = Idempotent. Sending the same PUT twice results in the same state.
- Your webhook handler must be idempotent even though it receives POST requests — because the sending service will retry on timeout. That's why you built the idempotency layer: to make POST behave like PUT at the application level.

---

## OOP — SOLID in a Code Snippet

**Q:** Identify which SOLID principle this code violates and fix it.

```js
class ReportGenerator {
  generateReport(data, format) {
    if (format === 'pdf') {
      // 50 lines of PDF generation
    } else if (format === 'csv') {
      // 50 lines of CSV generation
    } else if (format === 'excel') {
      // 50 lines of Excel generation
    }
  }
}
```

**Violation:** Open/Closed Principle — adding a new format requires modifying `ReportGenerator`.
**Fix:** Extract an interface `ReportFormatter` with a `format(data)` method. `PdfFormatter`, `CsvFormatter`, `ExcelFormatter` each implement it. `ReportGenerator` depends on the interface, not concrete classes.

---

## Git — Merge vs Rebase

**Q:** You're on a feature branch with 3 commits. Main has moved forward by 2 commits since you branched. Should you merge or rebase? Walk me through what the commit DAG looks like after each.

**Q (follow-up):** If two teammates are working on the same feature branch, what's the danger of rebasing and force-pushing?

*Expected:* Force-pushing a rebased branch rewrites commit history. If your teammate has already based work on those commits, their history diverges from the remote — they'll need to do a painful reset or rebase of their own branch.

---

## Security — SQL Injection + Password Hashing

**Q1:** Show me a vulnerable SQL query and explain how prepared statements fix it.

**Q2:** Why are passwords salted before hashing? What attack does a salt prevent?

*Expected:* A salt is a random value prepended to the password before hashing. Without it, identical passwords produce identical hashes — attackers can precompute a "rainbow table" of common password hashes and look up your hash directly. A salt makes every hash unique even for identical passwords, making precomputation infeasible.
