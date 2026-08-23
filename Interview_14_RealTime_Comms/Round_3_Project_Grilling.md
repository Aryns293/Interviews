# Round 3 — Past Experience & Project Grilling
**Interview:** Real-Time Communications / Streaming
**Duration:** 45 minutes
**Project in focus:** CodeSync AI (WebSockets, real-time collaboration).

---

## Q1 — CodeSync AI: The Scaling Problem

> "CodeSync AI uses Socket.IO. Right now, it works because all users in a 'room' connect to the same single Node.js process. What happens when you deploy 5 instances of the CodeSync backend behind a Load Balancer? How do User A and User B chat if User A is on Server 1 and User B is on Server 2?"

*What I'm listening for:*
- You must explain the need for a Pub/Sub Backplane.
- Socket.IO provides the `socket.io-redis` adapter (or you can build it yourself with Redis Pub/Sub).
- When Server 1 receives a message for Room X, it broadcasts it to its local clients AND publishes it to Redis channel `Room X`. Server 2 is subscribed to Redis channel `Room X`, receives the message, and broadcasts it to its local clients.

---

## Q2 — CodeSync AI: Disconnect & Reconnect

> "A user's internet drops for 5 seconds on a train, then reconnects. Socket.IO reconnects automatically. But what happened to the keystrokes their partner typed during those 5 seconds? How does your system reconcile the missed state?"

*Expected:*
- Basic Socket.IO does not buffer missed messages if the socket closes. The messages are lost.
- To fix this, you need a message history log on the server.
- The client reconnects and sends its `last_seen_message_id` or `version`. The server replays all events from that version onward. (This is exactly how Kafka or Slack works).

---

## Q3 — CodeSync AI: Execution Bottlenecks

> "You execute user code in a Docker sandbox. Running `docker run` takes hundreds of milliseconds just to start the container. If 10 users click 'Run Code' at once, does it block your WebSocket event loop?"

*Expected:*
- "No, because `docker run` is executed via `child_process.exec()` or `spawn()`, which is asynchronous in Node.js. It does not block the main thread."
- However, spinning up 10 containers simultaneously will spike CPU/Memory on the host. To fix this, you would need a worker pool of pre-warmed containers (like AWS Lambda does).
