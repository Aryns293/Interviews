# Round 0 — Online Assessment
**Interview:** B2B SaaS / Enterprise Software (e.g., Stripe, Twilio, Salesforce)
**Format:** 90 minutes. Emphasizes clean code, API design logic, and string parsing.

---

## DSA Problems

### Problem 1 — Serialize and Deserialize Binary Tree (Tree)
**Difficulty:** Hard

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

---

### Problem 2 — Valid Arrangement of Pairs (Eulerian Path)
**Difficulty:** Hard

You are given a 0-indexed 2D integer array pairs where pairs[i] = [starti, endi]. An arrangement of pairs is valid if for every index i where 1 <= i < pairs.length, we have endi-1 == starti. Return any valid arrangement of pairs.

---

## MCQs

### HTTP and API Design
**Q:** When building a public API for B2B customers, when should you return a `400 Bad Request` vs a `422 Unprocessable Entity`?
**A:** `400` means the JSON itself is malformed (syntax error, unparseable). `422` means the JSON is valid, but the business logic fails (e.g., passing a string into a field that expects an integer, or missing a required field).

### Idempotency
**Q:** Why does Stripe require an `Idempotency-Key` header on POST requests?
**A:** If the network drops after Stripe processes the payment but before the client receives the 200 OK, the client will retry. Without idempotency, the customer is double-charged. With it, Stripe sees the same key and returns the cached 200 OK response without re-running the charge.
