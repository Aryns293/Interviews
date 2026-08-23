# Round 4 — Applied Coding: Backend + Full-Stack
**Interview:** E-Commerce / High-Traffic Retail
**Duration:** 60 minutes

---

## Build 1 — Cart Total Calculator with Promotions (30 minutes)

### The Problem
Write a function that calculates the total price of a cart. 
It must apply a list of promotions. 
Promotions can be:
- `PERCENTAGE_OFF`: X% off the entire cart (if subtotal > Y).
- `BOGO`: Buy one get one free on specific item IDs.
- `FIXED_DISCOUNT`: Flat ₹Z off if the cart contains item ID W.

```js
function calculateTotal(cartItems, promotions) {
  // TODO: implement
}
```

**What I'm watching for:**
- Separation of concerns. Do you write one giant `if/else` block, or do you create an array of `PromotionRule` strategies that you reduce over the cart?
- **The Edge Case:** Order of application matters. Does a 10% discount apply *before* or *after* the flat ₹50 discount? You must ask the interviewer about the precedence rules before you code. E-commerce systems always define strict precedence (usually applying line-item discounts first, then cart-level flat discounts, then cart-level percentage discounts).

---

## Debug 1 — The Overselling Bug (15 minutes)

### The Problem
"We use this code to deduct inventory. Sometimes inventory drops below 0. Find the bug."

```js
async function purchaseItem(itemId, userId) {
  const item = await db.query('SELECT stock FROM inventory WHERE item_id = $1', [itemId]);
  if (item.stock > 0) {
    await db.query('UPDATE inventory SET stock = stock - 1 WHERE item_id = $1', [itemId]);
    return true;
  }
  return false;
}
```

**The Fix:**
- Classic Race Condition (Time of Check to Time of Use - TOCTOU).
- Fix 1: Wrap in transaction with `SELECT ... FOR UPDATE`.
- Fix 2 (Better): `UPDATE inventory SET stock = stock - 1 WHERE item_id = $1 AND stock > 0 RETURNING stock;`. If rows affected is 0, it failed.

---

## Discussion — Pagination at Scale (15 minutes)

> "A user searches for 'shoes'. There are 500,000 results. The user clicks page 5,000. `SELECT * FROM products WHERE category='shoes' LIMIT 20 OFFSET 100000;` is taking 5 seconds. Why, and how do you fix it?"

*Expected:*
- `OFFSET` forces the DB to scan and discard 100,000 rows. It is O(N) where N is the offset.
- **Fix:** Keyset pagination (Cursor-based pagination).
- `SELECT * FROM products WHERE category='shoes' AND id > last_seen_id ORDER BY id ASC LIMIT 20;`
- This uses the B-Tree index to jump directly to `last_seen_id` in O(log N) time.
