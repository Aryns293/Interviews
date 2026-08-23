# Round 5 — LLD & Light System Design
**Interview:** Remote-First / Fully Distributed Team Company

---

## LLD Problem — Async Ticket Board

> "Design a Task/Ticket board (Trello/Linear-style) with async status-change notifications, not real-time chat."

**Classes:**
```
class Ticket {
  String id
  String title
  Status status
  String assigneeId
  List<Comment> comments
  
  void changeStatus(Status newStatus, String userId)
}

enum Status { BACKLOG, IN_PROGRESS, REVIEW, DONE }

class Comment {
  String text
  String authorId
  long timestamp
}

// The crucial async part:
interface NotificationDispatcher {
  void notify(TicketEvent event)
}
```

**Design questions I'll ask:**
1. "When someone comments, how do you know who to notify?" (Need a list of `subscribers` or `watchers` on the Ticket).
2. "How do you prevent spamming someone who is asleep with 50 emails about 1 ticket?" (Batching/digests).

---

## Design Pattern Follow-Ups

**Observer — Async Pub/Sub:**
> "Teammates across timezones get notified when a ticket changes state. Why is pub/sub better than having the frontend poll?"

*Expected:* Polling wastes server resources and drains client battery. A pub/sub model (like Redis Pub/Sub feeding into WebSockets/SSE or pushing to an email queue) pushes the event exactly when it happens.

**Command — Audit Trail:**
> "Every status change is a reversible, logged action. Why?"

*Expected:* Because no one was watching live when it happened. If you wake up and a ticket is moved to "Done" incorrectly, you need an audit trail of *who* moved it and *when*, and you need the ability to "Undo" it cleanly. The Command pattern encapsulates an action as an object that can be logged and reversed.

---

## Thread-Safety — Async Conflict

**Q:**
> "Two teammates in different timezones update the same ticket's status within seconds of each other. How do you resolve the conflict?"

*Expected:* 
- **Last-write-wins (LWW):** Bad UX. One teammate's work is silently overwritten.
- **Optimistic Concurrency Control (OCC):** Add a `version` integer to the ticket. 
  - T1 reads version 1. T2 reads version 1.
  - T1 saves, version becomes 2.
  - T2 tries to save, passing `version = 1`. DB rejects it because current version is 2.
  - The UI tells T2: "This ticket was modified by someone else. Here are their changes. Do you want to overwrite?"

---

## Light System Design — Async Notification Digest

> "Design an async Notification/Digest system for a distributed team. Batches updates, respects working hours, avoids notification overload."

**Architecture:**
1. **Event Ingestion:** Microservices publish events (`TicketCreated`, `CommentAdded`) to Kafka/RabbitMQ.
2. **Notification Router:** Reads events. Checks user preferences (Immediate, Hourly, Daily).
3. **If Immediate:** Send to Push/Email service.
4. **If Batched:** Store event in a DB/Redis list keyed by `user_id`.
5. **Digest Worker (Cron):** Runs every hour. 
   - Checks which users are in their "active working hours" (timezone aware).
   - Pops all pending events for that user.
   - Aggregates them ("3 tickets were updated, 5 new comments").
   - Sends a single digest email.
