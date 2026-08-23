# Round 3 — Past Experience & Project Grilling
**Interview:** E-commerce / Marketplace Company
**Duration:** 45 minutes

---

## Q1
"QueueFlow's 3-tier priority engine — if this processed order-fulfillment jobs instead of generic ones, is 'priority' even the right lens, or does an order queue need strict FIFO per customer instead? Defend either choice."

## Q2
"Your dual-layer Redis/Postgres idempotency from Rolewize, applied directly here: two clicks on 'Place Order' within 200ms. What exactly stops a double charge?"

## Q3
"gitlight's content-addressable storage dedupes by hash — is there anything in that model actually reusable for a product-image storage pipeline, or is that a stretch?"

## Q4
"What breaks in QueueFlow if traffic goes from 1,000 concurrent jobs to 100,000 in an hour, Black-Friday style? Where's the first bottleneck?"
