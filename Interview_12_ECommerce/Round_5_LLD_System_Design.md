# Round 5 — LLD & Light System Design
**Interview:** E-Commerce / High-Traffic Retail

---

## LLD Problem — Payment Gateway Integration

> "Design a Payment Integration module. Users can pay with Credit Card, UPI, or Wallet. The payment processor's API is flaky and sometimes times out without confirming if the payment succeeded."

**Classes:**
```
interface PaymentStrategy {
  PaymentResponse charge(double amount, Map<String, String> details);
}
class UPIStrategy implements PaymentStrategy { ... }
class CreditCardStrategy implements PaymentStrategy { ... }

class PaymentService {
  PaymentResponse processPayment(Order order, PaymentStrategy strategy);
  void handleTimeout(String transactionId);
}
```

**Design questions I'll ask:**
1. "The API times out. Did the user get charged? What do you do?"
   *Expected:* You do NOT retry blindly (double charge risk). You transition the order to `PAYMENT_PENDING_CONFIRMATION`. You fire an async background job (QueueFlow) that polls the payment gateway's `GET /status/{transactionId}` endpoint every minute. If it eventually returns success, complete the order. If it returns fail, fail the order.

---

## Light System Design — E-Commerce Flash Sale

> "Design the backend for a 1-minute flash sale of 10,000 iPhones. Expecting 5 million active users."

**Architecture:**
1. **Frontend / CDN:** Serve a static "Waiting Room" page via CDN. Do not hit the dynamic API server for HTML rendering.
2. **Rate Limiting / Load Shedding (API Gateway):** 5 million requests hitting your backend will kill it. The API Gateway drops 99% of requests randomly (or via a fair lottery) and returns a 429 Too Many Requests before they even hit the application servers.
3. **Queueing (The 'QueueFlow' angle):** The 1% of users who pass the gateway do NOT go directly to the database. Their requests go into a Redis-backed queue.
4. **Workers:** A fixed pool of workers process the queue. They decrement inventory in Redis using an atomic Lua script.
5. **Database (Postgres):** The workers write the finalized orders to Postgres asynchronously. Postgres handles a smooth 1,000 writes/sec instead of a spiked 5 million writes/sec.

**Key constraint:** Separation of the "Intent to Buy" (fast, Redis, Queue) from the "Order Creation" (slow, Postgres).
