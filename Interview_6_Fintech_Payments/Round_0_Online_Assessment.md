# Round 0 — Online Assessment
**Interview:** Fintech / Payments Company
**Format:** Auto-graded — 90 minutes. MCQs are heavier on DBMS isolation levels and security than other OAs. This company cares deeply about correctness, not just speed.

---

## DSA Problems

### Problem 1 — Subarray Sum Equals K
**Difficulty:** Medium

Given an integer array `nums` and an integer `k`, return the total number of contiguous subarrays whose sum equals `k`.

**Fintech framing:** Find runs of consecutive transactions that sum to exactly a flagged amount (fraud pattern detection). The array is a daily ledger.

**What I'm testing:**
- Do you recognize the prefix sum + hashmap approach immediately? O(n) time, O(n) space.
- Can you explain *why* `prefix[j] - prefix[i] = k` means subarray `[i+1..j]` sums to k?
- Edge cases: negative numbers in the array (the sliding window approach breaks here — only prefix sum works), k = 0.

**Common mistake:** Using a sliding window (two-pointer) — that only works for positive numbers. Transactions can be negative (refunds).

---

### Problem 2 — Merge Intervals
**Difficulty:** Medium

Given an array of intervals `[start, end]`, merge all overlapping intervals and return the result.

**Fintech framing:** Merge overlapping settlement windows from multiple payment processors into a single consolidated clearing window.

**What I'm testing:**
- Sort by start time first — do you state this before coding?
- Correct merge condition: `current.start <= last.end` → merge
- Edge cases: single interval, all intervals overlapping, no overlaps

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
