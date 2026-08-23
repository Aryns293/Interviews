# Round 0 — Online Assessment
**Interview:** Fintech / Payments Company
**Format:** Auto-graded — 90 minutes. MCQs are heavier on DBMS isolation levels and security than other OAs. This company cares deeply about correctness, not just speed.

---

## DSA Problems

### Problem 1 — Split Array Largest Sum (Binary Search)
**Difficulty:** Hard

Given an integer array nums and an integer k, split nums into k non-empty subarrays such that the largest sum of any subarray is minimized. Return the minimized largest sum of the split.

---

### Problem 2 — Number of Submatrices That Sum to Target (Prefix Sum)
**Difficulty:** Hard

Given a matrix and a target, return the number of non-empty submatrices that sum to target. A submatrix x1, y1, x2, y2 is the set of all cells matrix[x][y] with x1 <= x <= x2 and y1 <= y <= y2.

---

## MCQ Section

### DBMS — Isolation Level Identification
**Scenario:**
> Transaction T1 reads account balance = ₹10,000. Transaction T2 then debits ₹8,000 and commits. T1 reads again and sees ₹2,000, even though T1 hasn't committed yet.

**Question:** What anomaly is this, and which isolation level prevents it?
- **Anomaly:** Non-repeatable read
- **Prevented by:** `REPEATABLE READ` or `SERIALIZABLE`

### Bitwise Output Prediction
```c
unsigned int x = 0xFF;      // 11111111
unsigned int mask = 0x0F;   // 00001111
printf("%d", (x & mask) | (x >> 4));
// Expected: 15 | 15 = 15
```

### DBMS — Normalization on a Transactions Schema
Given:
```
payments(payment_id, user_id, user_email, merchant_id, merchant_name, amount, timestamp)
```
FDs: `payment_id → all`, `user_id → user_email`, `merchant_id → merchant_name`

**Q:** What normal form is this in, and what's the violation?
- Currently in **2NF** (no partial dependencies on a composite key)
- Violation of **3NF**: transitive dependencies `payment_id → user_id → user_email` and `payment_id → merchant_id → merchant_name`
- **Fix:** Split into `payments(payment_id, user_id, merchant_id, amount, timestamp)`, `users(user_id, email)`, `merchants(merchant_id, name)`
