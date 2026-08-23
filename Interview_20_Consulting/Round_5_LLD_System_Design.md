# Round 5 — LLD & Light System Design
**Interview:** Consulting / Client-Services Software Company
**Duration:** 60 minutes

---

## LLD — Booking System (Underspecified)
- Deliberately underspecified — "design a booking system," nothing further given up front.
- You're expected to ask clarifying questions before designing anything (restaurant? equipment rental? meeting rooms?) — the point is what you ask, not just what you build.
- **Pattern:** Factory (swap notification providers — email today, SMS next quarter — without touching business logic) → Adapter (integrating a legacy client system with an incompatible interface without rewriting either side).
- **Thread-safety:** The standard double-booking race on the system you just designed.

---

## Light HLD — Scaling an Underspecified System
"A client says 'make it scale' with zero further detail"
- What do you ask before designing anything?
- What's a reasonable default architecture if they can't answer most of it?
