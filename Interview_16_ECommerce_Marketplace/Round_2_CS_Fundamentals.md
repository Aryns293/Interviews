# Round 2 — CS Fundamentals
**Interview:** E-commerce / Marketplace Company
**Duration:** 45 minutes

---

## DBMS — Overselling & Indexing
- A concrete overselling scenario — two customers buy the last unit simultaneously — which isolation level or locking strategy prevents it.
- B+Tree rationale on a category + price-range catalog index.

---

## Computer Networks
- Cache-Control/ETag for a product listing page.
- CDN vs origin.

---

## Redis
- Cache-aside for "get product details."
- **Cache stampede:** What happens when a popular product's cache entry expires under heavy concurrent read load, and how do you stop a thundering herd hitting Postgres at once?

---

## Security
- Why you never trust a client-submitted price or discount value — the actual exploit if you did.

---

## SQL
- Top 5 best-selling products per category using window functions.


---

## Exhaustive CS Fundamentals (Theoretical & SQL)
*These are additional deep-dive topics specifically assigned to this interview loop to ensure 100% breadth coverage across all 20 interviews.*

### OS - File Systems
**Links:** Hard Links vs Soft (Symbolic) Links: What is the difference? What happens to the link if the original file is deleted?

### CN - APIs
**REST vs gRPC:** Why would an enterprise backend use gRPC for internal microservices but expose a REST API to clients?

### SQL - Query 6
**Pivoting Rows to Columns:** Output names sorted alphabetically, pivoted so that each occupation has its own column (using `CASE WHEN` combined with aggregates like `MAX()`).

