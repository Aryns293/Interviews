# Round 5 — LLD & Light System Design
**Interview:** E-commerce / Marketplace Company
**Duration:** 60 minutes

---

## LLD — Shopping Cart & Checkout System
- Cart, CartItem, Inventory hold/reservation, Order, PaymentAttempt.
- **Pattern:** Strategy (pluggable discount/promo rules) → Observer (inventory-level change notifying a low-stock alert subsystem).
- **Thread-safety:** 50 concurrent buyers, 3 units left — guarantee no overselling without serializing everyone behind one global lock.

---

## Light HLD — Product Search & Catalog System
"Design a Product Search & Catalog system"
- Inverted-index-style search.
- Faceted filtering.
- Read-heavy caching.
- How catalog updates propagate to the search index without a long staleness window.
