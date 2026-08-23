# Round 7 — Hiring Manager / Bar Raiser
**Interview:** Real-Time Communications / Streaming

---

## Resume Defense — The WebSocket Choice

> "You built CodeSync AI with WebSockets. If I told you tomorrow we are rewriting it to use HTTP Long Polling instead, how hard would you fight me? Why?"

*What I'm testing:* Pragmatism vs dogma.
*Strong answer:* "I would fight it strongly, but I would listen. If you told me our corporate firewall blocks WebSockets on port 443 for 50% of our enterprise clients, then Long Polling is the only pragmatic choice to ensure deliverability. But purely from an engineering standpoint, Long Polling forces us to establish a new TCP connection and TLS handshake for every single keystroke. That adds 100ms+ of overhead to every edit and will destroy our server's CPU with connection churn. I would propose WebSockets as primary, with Long Polling as an automatic fallback."

---

## Judgment Scenario — The Memory Leak

> "Our Node.js WebSocket server is crashing every 4 hours with an Out Of Memory (OOM) error. You have to fix it. Walk me through your debugging steps in production."

*Expected:*
1. **Identify:** A WebSocket server OOM is almost always caused by holding onto references of disconnected sockets.
2. **Action:** Take a heap snapshot (`v8.getHeapSnapshot()`). Load it into Chrome DevTools Memory tab.
3. **Analyze:** Look for objects with a high "Retained Size". Usually, it's an array or a Map of `connectedUsers` where we forgot to call `connectedUsers.delete(socket.id)` inside the `socket.on('disconnect')` handler.
4. **Fix:** Ensure cleanup logic runs on *every* close event (graceful or ungraceful network drops).

---

## Close

**Q1:** "What's the hardest concurrency bug you've ever had to track down?"
*(Have a story ready about race conditions, like a delayed webhook hitting exactly when a user was modifying their data).*

**Q2:** "Questions for me?"
