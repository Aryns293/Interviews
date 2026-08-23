# Round 5 — LLD & Light System Design
**Interview:** Site Reliability Engineering (SRE) / DevOps-Focused Company
**Duration:** 60 minutes

---

## LLD — Alerting & On-Call Escalation System
- Alert, Severity, EscalationPolicy, OnCallSchedule, Acknowledgment, auto-escalation if unacknowledged within N minutes.
- **Pattern:** Observer (alert rules reacting to metric streams) → State (incident lifecycle: Triggered → Acknowledged → Resolved → Postmortem).
- **Thread-safety:** Two on-call engineers acknowledge the same alert within seconds — avoid double-paging a third engineer due to an escalation race.

---

## Light HLD — Monitoring & Alerting Pipeline
"Design a Monitoring & Alerting pipeline" for a fleet like your own three projects.
- Metric ingestion.
- Threshold/anomaly alerting.
- Specifically avoiding alert fatigue.
