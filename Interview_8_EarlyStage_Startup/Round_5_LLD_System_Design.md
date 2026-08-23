# Round 5 — LLD & Light System Design
**Interview:** Early-Stage Seed Startup

---

## LLD Problem — Extensible Tic-Tac-Toe

> "Design Tic-Tac-Toe. But requirements will change constantly here. Build it for an N×N board with pluggable win conditions from day one, not hardcoded 3×3 logic."

**What I'm testing:** Do you over-engineer or under-engineer?
- Over-engineering: Abstract `Cell` classes, complex event buses for simple turns.
- Under-engineering: `board[3][3]` arrays with hardcoded `board[0][0] == board[0][1]` checks.
- Just right: A generic grid, generic player enums, and a strategy-based win checker.

**Classes:**
```
enum Player { NONE, X, O }

class Board {
  int size
  Player[][] grid
  // O(1) checks for N*N board:
  int[] rowCounts
  int[] colCounts
  int diagCount
  int antiDiagCount
  
  boolean place(int r, int c, Player p)
}

interface WinStrategy {
  boolean checkWin(Board b, int lastMoveRow, int lastMoveCol, Player p);
}
// Concrete strategies: StandardWinStrategy (N in a row), CornerWinStrategy (all 4 corners), etc.

class Game {
  Board board
  List<WinStrategy> strategies
  Player currentTurn
  
  boolean makeMove(int r, int c)
}
```
*Crucial optimization:* The O(1) row/col/diag count arrays for checking N-in-a-row without scanning the whole board.

---

## Design Pattern Follow-Ups

**Factory — Payment Integrations:**
> "You integrate Stripe today; the founder wants to test Razorpay next month. Which pattern keeps your checkout code from caring which one's active?"

*Expected:* Factory Pattern.
```js
interface PaymentProcessor { charge(amount); }
class StripeProcessor implements PaymentProcessor { ... }
class RazorpayProcessor implements PaymentProcessor { ... }

class PaymentFactory {
  static getProcessor(type): PaymentProcessor {
    if (type === 'stripe') return new StripeProcessor();
    if (type === 'razorpay') return new RazorpayProcessor();
  }
}
```

**Observer — Analytics:**
> "Design a lightweight analytics pipeline reacting to user actions."

*Expected:* Observer Pattern. User actions emit events (`authService.emit('signup', user)`). An analytics observer listens and pushes to Mixpanel/PostHog asynchronously without blocking the main request flow.

---

## Thread-Safety — Beta Waitlist Concurrency

**Q:**
> "You're running a 50-spot beta waitlist. Two signups land in the same second for the last spot. You don't have time to build a full distributed lock with Redis. How do you avoid over-allocating?"

*Expected:* Use the database you already have.
```sql
-- Atomic update with condition
UPDATE waitlist_config 
SET available_spots = available_spots - 1 
WHERE id = 1 AND available_spots > 0;
```
If the update affects 0 rows, the spot is gone. Transactional, atomic, requires zero new infrastructure.

---

## Light System Design — MVP Notification System

> "Design an MVP Notification System (email + push) that has to ship in a week. What's the 20% of design effort that covers 80% of what you'll actually need in 6 months, without painting yourself into a corner?"

**The 20% that matters:**
1. **Decoupled triggering:** Don't write `sendEmail()` inside your `signup()` function. Fire a `UserSignedUp` event into a basic pub/sub (even Redis PUB/SUB or an in-memory queue if single-instance).
2. **Abstracted providers:** `EmailProvider` interface wrapping SendGrid. Don't hardcode SendGrid API calls across the codebase.
3. **Idempotency/dedup:** A simple table `sent_notifications (user_id, event_id, timestamp)` to ensure you don't double-email someone if the worker retries.

**What to skip for the MVP:**
- User-configurable preference center (opt in/out per notification type) — just give them a global unsubscribe link initially.
- Batching/digests — send immediately for now.
- Deep analytics (open tracking) — use the provider's built-in dashboard.
