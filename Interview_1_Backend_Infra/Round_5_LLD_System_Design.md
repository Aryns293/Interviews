# Round 5 — LLD & Light System Design
**Interview:** Backend / Infra-Heavy Product Company
**Duration:** 60–75 minutes
**Format:** Whiteboard (or shared doc). I'll give you one LLD problem to work to actual classes, ask about design patterns, then close with a system design that maps directly to your project work.

---

## LLD Problem — Design Splitwise

**The ask:** Design the core class model for a shared expense splitting application.

**Requirements:**
- Users can create groups
- Users can add expenses to a group
- Expenses can be split equally, by percentage, or by exact amounts
- The system must calculate the simplified balance for each user (who owes whom, net)
- Users can settle debts

### What I expect by the end of this session

```
Classes:
- User(id, name, email)
- Group(id, name, members: User[])
- Expense(id, description, amount, paidBy: User, split: Split)
- Split (abstract/interface)
  - EqualSplit implements Split
  - PercentageSplit implements Split
  - ExactSplit implements Split
- Balance(from: User, to: User, amount: float)
- ExpenseManager (orchestrator — adds expenses, computes balances)
```

**Walk me through your design decisions out loud.** I'm not just grading the output — I'm grading how you think.

---

### Design Pattern Follow-up — Strategy Pattern

**Q:** What design pattern are you using for the split logic (Equal/Percentage/Exact)?

*Expected:* Strategy Pattern — define a family of algorithms (split strategies), encapsulate each one, and make them interchangeable.

**Q:** If I added a new split type tomorrow — say, split by income ratio — what's the exact change you'd need to make to your codebase?

*Expected:* Create a new class `IncomeRatioSplit implements Split`. Zero changes to `Expense` or `ExpenseManager`. Open/Closed Principle satisfied.

---

### Thread-Safety Follow-up

**Q:** Two users settle a debt simultaneously. User A settles ₹500 with User B, and User B settles ₹300 with User A at the same moment. What happens to their balance? Is it correct?

*What I want to hear:*
- Without synchronization, both reads happen before either write, causing a race condition
- The fix: a per-user-pair lock (or optimistic locking with versioning in a database context)
- In a distributed system: use a Redis-based distributed lock or a database transaction with `SELECT FOR UPDATE`

**Direct tie to your project:**
> "Two BullMQ workers pick up the same job ID simultaneously. You claim QueueFlow prevents this. Walk me through the exact locking mechanism — not 'Redis handles it,' but the actual Redis commands and the atomicity guarantee."

*Expected:* `SET job:<id>:lock <worker_id> NX PX 30000` — atomic SET only if Not eXists, with a 30-second expiry. The worker ID in the value prevents another worker from releasing a lock it doesn't own.

---

## Light System Design — Webhook Delivery System

**The ask:** Design a production webhook delivery system. A platform (like Rolewize) needs to reliably deliver events to thousands of third-party subscriber endpoints.

**Scope:** You have 30 minutes. I'm not expecting a 6-hour architecture document. I want to see your reasoning.

### What I'll ask you to cover

**Ingestion layer:**
- How do events get into the system? (API endpoint → event queue)
- How do you handle a burst of 10,000 events in 1 second?

**Delivery layer:**
- How do you guarantee at-least-once delivery?
- What happens when a subscriber endpoint is down?

**Retry & backoff:**
- Exponential backoff with what ceiling?
- After how many failures do you move to a dead-letter queue?

**Signature verification:**
- How does a subscriber verify the event is authentic? (HMAC — you've built this)

**Exactly-once vs at-least-once:**
- Is exactly-once delivery even possible here? What would it require? (Idempotency key on the subscriber side — you've built this too)

### What I'm listening for

| Component | Pass | Fail |
|---|---|---|
| Event ingestion | Uses a queue buffer (Redis/Kafka) to absorb spikes | Writes directly to subscriber endpoints synchronously |
| Retry logic | Exponential backoff with a dead-letter queue | "Just retry 3 times" with no backoff |
| Signature | HMAC-SHA256, timing-safe comparison | "Add an API key" — not the same thing |
| Exactly-once | Honest about the impossibility, explains idempotency key pattern | Claims exactly-once with no mechanism |
| Data at scale | Horizontal scaling of delivery workers | Single process handling all deliveries |
