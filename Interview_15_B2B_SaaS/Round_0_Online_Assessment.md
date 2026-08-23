# Round 0 — Online Assessment
**Interview:** B2B SaaS / Enterprise Software (e.g., Stripe, Twilio, Salesforce)
**Format:** 90 minutes. Emphasizes clean code, API design logic, and string parsing.

---

## DSA Problems

### Problem 1 — Design Hit Counter
**Difficulty:** Medium

Design a hit counter which counts the number of hits received in the past 5 minutes (300 seconds).

**SaaS framing:** API Rate Limiting core logic.

**What I'm testing:**
- Can you design this for scale?
- *Approach 1 (Basic):* A Queue. Push timestamp. On query, pop timestamps older than `now - 300`. Return queue length. (Bad for memory if 1M hits/sec).
- *Approach 2 (Buckets):* Array of size 300 (one per second). Array 1 stores timestamps, Array 2 stores counts. Map `time % 300` to index. If `timestamp[idx] == current_time`, increment count. Else, reset `timestamp[idx] = current_time` and `count = 1`. Sum the counts array for the answer. O(1) time and space.

---

### Problem 2 — Text Justification
**Difficulty:** Hard

Given an array of words and a width `maxWidth`, format the text such that each line has exactly `maxWidth` characters and is fully (left and right) justified.

**SaaS framing:** Report generation, PDF formatting, string manipulation for billing statements.

**What I'm testing:**
- Extreme attention to detail.
- Handling the greedy packing of words per line.
- Calculating the exact number of spaces between words `(maxWidth - totalChars) / (numWords - 1)`.
- Handling the uneven distribution of spaces (left slots get extra).
- Handling the edge case of the last line (left justified).

---

## MCQs

### HTTP and API Design
**Q:** When building a public API for B2B customers, when should you return a `400 Bad Request` vs a `422 Unprocessable Entity`?
**A:** `400` means the JSON itself is malformed (syntax error, unparseable). `422` means the JSON is valid, but the business logic fails (e.g., passing a string into a field that expects an integer, or missing a required field).

### Idempotency
**Q:** Why does Stripe require an `Idempotency-Key` header on POST requests?
**A:** If the network drops after Stripe processes the payment but before the client receives the 200 OK, the client will retry. Without idempotency, the customer is double-charged. With it, Stripe sees the same key and returns the cached 200 OK response without re-running the charge.
