# Round 5 — LLD & Light System Design
**Interview:** B2B SaaS / Enterprise Software

---

## LLD Problem — Webhook Dispatcher System

> "Design the core classes for Rolewize's Webhook Dispatcher. It must handle retries, exponential backoff, and signature generation."

**Classes:**
```
class WebhookEvent {
  String id;
  String eventType; // e.g., "user.created"
  JSONObject payload;
}

class Endpoint {
  String url;
  String secretKey;
}

class WebhookDispatcher {
  SignatureGenerator signer;
  QueueClient queue;
  
  void dispatch(WebhookEvent event, Endpoint target);
}

class SignatureGenerator {
  String generateHMAC(String secret, String payload);
}

// Queue Worker
class WebhookWorker {
  HttpClient client;
  
  void processJob(Job job) {
    try {
      Response res = client.post(job.url, job.payload, job.headers);
      if (res.status >= 500) throw new ServerError();
    } catch (Exception e) {
      handleRetry(job);
    }
  }
}
```

**Design questions I'll ask:**
1. "If `client.post()` blocks indefinitely because the customer's server accepts the TCP connection but never sends an HTTP response, what happens to your worker?"
   *Expected:* The worker thread hangs forever. You MUST configure a strict HTTP client timeout (e.g., 5000ms) on all outbound webhook requests.

---

## Light System Design — API Gateway Rate Limiter (Stripe Scale)

> "Design a distributed Rate Limiter for an API processing 100,000 requests per second across 50 regional edge nodes."

**Architecture:**
- **Local vs Global:** A single Redis instance cannot handle 100k writes/sec for rate limiting globally across all regions (latency + throughput limits).
- **Two-Tiered Approach:**
  1. **Local Node Cache (In-Memory):** Each edge node (e.g., in US-East) keeps a local counter for an API key. 
  2. **Global Sync (Redis/Cassandra):** Every X seconds, the edge nodes asynchronously flush their local counts to the global datastore, and pull the latest global counts.
- **Trade-off:** This is *eventually consistent*. A user might burst slightly over their limit for a few seconds before the global state syncs. In B2B SaaS, this slight overage is acceptable compared to the massive latency penalty of doing a synchronous global DB check on every single API request.
