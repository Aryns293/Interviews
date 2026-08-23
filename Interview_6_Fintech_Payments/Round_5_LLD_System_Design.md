# Round 5 — LLD & Light System Design
**Interview:** Fintech / Payments Company

---

## LLD Problem — ATM / Bank Account System

> "Design the core class model for an ATM system. Include: Account, Card, PIN validation, Transaction, overdraft rules, and daily withdrawal limits."

**Classes to design:**
```
Account(id, balance, overdraftLimit, dailyWithdrawalLimit)
  - debit(amount): void — throws if balance - amount < -overdraftLimit
  - credit(amount): void

Card(cardNumber, pin, account: Account)
  - validatePIN(input: string): boolean
  - isBlocked: boolean (after 3 failed attempts)

Transaction(id, account, type: DEBIT|CREDIT|REFUND, amount, timestamp, status)

DailyLimitTracker(account, date)
  - amountWithdrawnToday: float
  - canWithdraw(amount): boolean
  - record(amount): void

ATMController (orchestrator)
  - insertCard(card): void
  - enterPIN(pin): boolean
  - withdraw(amount): Transaction
  - deposit(amount): Transaction
  - ejectCard(): void
```

**Design questions I'll ask:**
1. "Where does the daily limit check live — on `Account`, on `ATMController`, or in a separate `DailyLimitTracker`? What's the tradeoff?"
2. "After 3 wrong PINs, the card is blocked. Where does that state live — in the Card, in a DB, or in Redis? What happens if the ATM crashes mid-session?"
3. "How do you prevent the ATM from dispensing cash before the DB confirms the debit?"

---

## Design Pattern Follow-Ups

**State Pattern — Transaction Lifecycle:**
> "Model the lifecycle of a payment transaction using the State pattern."

```
TransactionState (interface): execute(), refund(), cancel()

PendingState     → can transition to: Processing, Cancelled
ProcessingState  → can transition to: Completed, Failed
CompletedState   → can transition to: Refunded
FailedState      → terminal
RefundedState    → terminal
```

**Command Pattern — Refund as Reversible Operation:**
> "Why is a refund well-modeled as a Command pattern, not just a function call?"

*Expected:* Command pattern encapsulates the refund as an object with `execute()` + `undo()` + metadata (who issued it, when, for what amount). This creates a natural audit trail. Every refund command is stored. `undo()` re-debits the refunded amount. Essential for fintech audit compliance.

---

## Thread-Safety — Double Refund Prevention

**Q:**
> "Two support agents both click 'Refund' on the same transaction within milliseconds of each other. How do you guarantee only one refund actually happens?"

*Expected:*
```sql
-- Optimistic locking approach
UPDATE transactions
SET status = 'refunded', refund_initiated_at = NOW()
WHERE id = ? AND status = 'completed';
-- If 0 rows updated → already refunded, return error
```
- The atomic `UPDATE ... WHERE status = 'completed'` acts as a compare-and-swap
- First agent's update succeeds (1 row affected)
- Second agent's update fails (0 rows affected — status is now 'refunded')
- No distributed lock needed — the DB transaction handles it

---

## Light System Design — Idempotent Payment Webhook Ingestion

> "Design a payment webhook ingestion system. Requirements: at-least-once delivery from the processor, but exactly-once processing on our side. No double-charges, no missed events."

**Key distinction to nail:**
- **At-least-once delivery:** The payment processor will retry if we return a non-200. Guaranteed at least one delivery, possibly multiple.
- **Exactly-once processing:** Our system must process the business logic (credit the account) exactly once, even if the webhook arrives 3 times.

**Architecture:**
```
Payment Processor
       ↓ HTTPS POST (with HMAC-SHA256)
Webhook Ingestion API
  1. Verify HMAC signature → reject if invalid (401)
  2. Check idempotency: SET event_id NX in Redis
     - If NX fails → already seen, return 200 immediately (don't re-process)
     - If NX succeeds → mark as 'processing'
  3. Enqueue to BullMQ for async processing
  4. Return 200 immediately (prevents processor timeout + retry storm)
       ↓
BullMQ Worker
  5. Dequeue event
  6. Check Postgres: has this event_id already been processed?
     - Yes → skip (dual-layer idempotency)
     - No → BEGIN TRANSACTION
  7. Apply business logic (credit account, update ledger)
  8. Insert event_id into processed_events table
  9. COMMIT
```

**Why dual-layer (Redis + Postgres)?**
- Redis (fast path): catches 99% of duplicates before they queue, sub-millisecond check
- Postgres (durable path): catches the rare case where Redis was down or TTL expired
- The Postgres check inside the transaction ensures atomicity — business logic and dedup record are committed together

**Ordering guarantees:**
- Payment processors do NOT guarantee delivery order. Events from the same payer may arrive out of order.
- Solution: each event carries a sequence number or timestamp. The worker checks "does applying this event make sense given current state?" before committing.
