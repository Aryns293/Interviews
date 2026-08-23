# Round 5 — LLD & Light System Design
**Interview:** General Product-Based Company

---

## LLD Problem — Library Management System

> "Design the core class model for a library management system."

**Entities to design:**
```
Book(isbn, title, author, totalCopies, availableCopies)
Member(id, name, email, activeLoans: Loan[])
Loan(id, book: Book, member: Member, dueDate, returnDate)
Reservation(id, book: Book, member: Member, requestedAt)
FineCalculator(loan: Loan): float
LibraryManager (orchestrator)
  - checkout(member, book): Loan
  - returnBook(loan): void
  - reserve(member, book): Reservation
  - calculateFine(loan): float
```

**Design questions I'll ask:**
- Where does the "is this book available?" check live — on `Book`, on `LibraryManager`, or in the `checkout()` method?
- If a book is unavailable, does `checkout()` throw, return null, or create a reservation? What's the API contract?
- How does `FineCalculator` get the per-day fine rate — hardcoded, injected, or configurable?

---

## Design Pattern Follow-Ups

**Factory:**
> "Different types of members (Student, Faculty, Guest) have different loan limits and fine rates. How would you use the Factory pattern to create the right member type?"

```
MemberFactory.create(type: "student" | "faculty" | "guest"): Member
```

**Singleton:**
> "There should only ever be one `FineCalculator` instance in the system. How do you enforce that?"

*Expected:* Singleton — private constructor, static instance, public `getInstance()` method.

---

## Thread-Safety — Last Copy Race Condition

**Q:**
> "Two members try to reserve the last available copy of a book at the exact same time. How do you guarantee only one succeeds?"

**Option 1 — Application layer:**
- Optimistic locking: read `availableCopies`, check > 0, then update with `WHERE availableCopies > 0 AND version = <seen-version>`. If 0 rows updated, retry.

**Option 2 — Database layer:**
```sql
BEGIN;
SELECT availableCopies FROM books WHERE isbn = ? FOR UPDATE;
-- Only one transaction holds this lock at a time
UPDATE books SET availableCopies = availableCopies - 1 WHERE isbn = ?;
INSERT INTO loans ...;
COMMIT;
```

---

## Light System Design — URL Shortener

> "Design a URL shortener. I want the high-level architecture, your ID generation strategy, and your caching strategy."

**ID Generation:**
- Hash-based: MD5/SHA-256 the long URL → take first 7 chars. Risk: collision with different URLs producing same prefix.
- Counter-based: global atomic counter, encode in base62. No collisions. Risk: predictable/enumerable IDs, single point of failure for the counter.
- Hybrid: counter per shard, encode shard ID + counter. Best of both.

**Read path (heavy):**
```
Browser → CDN (edge cache, TTL ~1hr) → Load Balancer → App Server → Redis (short_url → long_url, TTL 24h) → DB
```

**Write path (light):**
```
Client → App Server → DB write → Redis invalidate/set
```

**Collision handling (hash-based):** append random suffix and retry until unique.

**Analytics:** async — don't block the redirect. Write click event to Kafka/SQS, consume in a separate analytics service.
