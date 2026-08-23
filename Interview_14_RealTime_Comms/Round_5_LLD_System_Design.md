# Round 5 — LLD & Light System Design
**Interview:** Real-Time Communications / Streaming

---

## LLD Problem — Chat Room Message Broker

> "Design a lightweight in-memory Message Broker (like a mini Redis Pub/Sub) to handle chat rooms."

**Classes:**
```
class MessageBroker {
  // Map of ChannelName -> Set of Subscribers
  Map<String, Set<Subscriber>> channels;
  
  void subscribe(String channel, Subscriber sub);
  void unsubscribe(String channel, Subscriber sub);
  void publish(String channel, Message msg);
}

interface Subscriber {
  void onMessage(Message msg);
}

class WebSocketConnection implements Subscriber {
  // implements onMessage to send data over the socket
}
```

**Design questions I'll ask:**
1. "If a channel has 100,000 subscribers, `publish()` loops over a Set of 100,000 objects sequentially. This will block the thread for milliseconds, introducing jitter. How do you fix it?"
   *Expected:* Do not do it sequentially on one thread. Shard the subscribers across a thread pool, or use a hierarchical fan-out (publish to 10 'regional' brokers, which each publish to 10k clients).

---

## Light System Design — Live Streaming Chat (Twitch Scale)

> "Design Twitch Chat. 1 million concurrent users watching the finale of a tournament, all typing 'Pog' in the same chat room."

**Architecture:**
- **The Problem:** 1M users typing 1 message a second = 1M writes/sec. Broadcasting 1M messages back to 1M users = 1 Trillion reads/sec. The system will melt.
- **The Solution (Fan-in / Sampling):** You CANNOT broadcast every message to every user.
1. **Sharding:** Users are split across hundreds of WebSocket edge servers.
2. **Aggregator / Sampler:** When edge servers receive a message, they don't forward all of them. They sample them (e.g., allow max 100 msgs/second per channel). The rest are dropped or aggregated ("10,000 users sent Pog").
3. **Redis Pub/Sub:** The sampled messages hit Redis Pub/Sub, which broadcasts to the edge servers.
4. **Broadcast:** The edge servers push the throttled 100 msgs/second down to the clients.
- **Persistence:** Write messages to Cassandra asynchronously in batches for chat replay.
