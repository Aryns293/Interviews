# Round 4 — Applied Coding: Backend + Full-Stack
**Interview:** Real-Time Communications / Streaming
**Duration:** 60 minutes

---

## Build 1 — Leaky Bucket Rate Limiter (30 minutes)

### The Problem
In real-time systems, traffic spikes are sudden. Write a class that implements the Leaky Bucket algorithm. 
- It has a `capacity` (max burst).
- It leaks at a constant `rate` (requests per second allowed).
- `allowRequest(bytes)` returns true if there is space in the bucket, false otherwise.

```js
class LeakyBucket {
  constructor(capacity, leakRatePerSec) { ... }
  allowRequest(size) { ... }
}
```

**What I'm watching for:**
- You DO NOT run an actual `setInterval` to leak the bucket (that wastes CPU).
- **The trick:** You calculate the leak dynamically at the time `allowRequest` is called.
- `timePassed = now - lastLeakTime;`
- `leakedAmount = timePassed * leakRate;`
- `currentLevel = max(0, currentLevel - leakedAmount);`
- `lastLeakTime = now;`

---

## Debug 1 — The React WebSocket Re-render Loop (15 minutes)

### The Problem
"A user connects to our chat. The WebSocket receives a heartbeat 'ping' every 1 second. The UI freezes because React is re-rendering the entire massive DOM tree every 1 second."

**The Fix:**
- The heartbeat is likely triggering a state update at the root context provider: `setLastPing(Date.now())`.
- If the whole app consumes this context, the whole app re-renders.
- **Fix:** Move the WebSocket listener *down* the tree to only the component that needs it (e.g., a small "Online Status" indicator), OR use a ref `useRef` to store the ping without triggering a re-render if the UI doesn't actually need to paint it.

---

## Discussion — WebSockets vs gRPC (15 minutes)

> "When would you choose gRPC over WebSockets for server-to-server real-time communication?"

*Expected:*
- WebSockets transmit unstructured text (or binary). You have to manually serialize/deserialize JSON, which is slow.
- gRPC uses HTTP/2 (multiplexing streams over a single connection) and Protocol Buffers (Protobufs). Protobufs are binary, strictly typed, and incredibly fast to serialize.
- For backend microservices talking to each other, always use gRPC. WebSockets are strictly for Browser-to-Server.
