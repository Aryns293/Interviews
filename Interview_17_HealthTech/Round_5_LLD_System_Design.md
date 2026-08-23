# Round 5 — LLD & Light System Design
**Interview:** HealthTech / Compliance-Heavy Company
**Duration:** 60 minutes

---

## LLD — Patient Record System
- Patient, Provider, Visit, Consent (who's consented to what data use), field-level access control as a first-class concept.
- **Pattern:** Decorator (audit logging wrapped around data access without touching business logic) → Strategy (pluggable consent-check rules).
- **Thread-safety:** Two providers update the same patient's record concurrently — guarantee neither silently overwrites the other, and both are auditable.

---

## Light HLD — Consent & Data-Retention Management System
"Design a Consent & Data-Retention Management system"
- Track what a user consented to.
- Auto-expire/delete per policy.
- Produce a compliance report on demand without scanning your entire production DB live.
