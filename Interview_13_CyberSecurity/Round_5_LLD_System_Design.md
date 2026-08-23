# Round 5 — LLD & Light System Design
**Interview:** Cyber Security / Compliance-Heavy Company

---

## LLD Problem — Rate Limiter with Security Thresholds

> "Design a Rate Limiter. Standard sliding window log is fine. But add a security requirement: if an IP address violates the rate limit 3 times in an hour, it gets added to a 'Penalty Box' and blocked entirely for 24 hours."

**Classes / Entities:**
```
class RateLimiter {
  int windowMs
  int maxRequests
  
  boolean isAllowed(String ip, long timestamp)
}

class SecurityMonitor {
  int violationThreshold // e.g., 3
  int penaltyDurationMs // e.g., 24h
  
  void recordViolation(String ip)
  boolean isBlacklisted(String ip)
}

class ApiGateway {
  RateLimiter limiter
  SecurityMonitor monitor
  
  void handleRequest(Request req) {
    String ip = req.getIp();
    if (monitor.isBlacklisted(ip)) throw new 403_Forbidden();
    
    if (!limiter.isAllowed(ip, req.getTime())) {
      monitor.recordViolation(ip);
      throw new 429_TooManyRequests();
    }
    // route request
  }
}
```

**Design questions I'll ask:**
1. "Where does the SecurityMonitor store the penalty box? If it's in Redis, what happens if an attacker spams millions of spoofed IP addresses (DDoS)? Your Redis fills up and OOMs."
   *Expected:* You must limit the size of the blocklist (e.g., max 100k IPs, evicting the oldest via LRU) or use a probabilstic data structure like a Bloom Filter or Count-Min Sketch to track violations without allocating memory per IP.

---

## Light System Design — Audit Logging System

> "Design a tamper-evident Audit Logging System. When an admin changes a user's permissions, we must log it. We need mathematical proof that the logs have not been altered or deleted by a rogue sysadmin after the fact."

**Architecture:**
- **Standard Logging:** Emit log event -> Kafka -> Elasticsearch. (Easily tampered with by anyone with DB access).
- **Tamper-Evident Addition (Hash Chaining):** Like a blockchain, every log entry includes the hash of the *previous* log entry. 
  `Entry[N].hash = SHA256(Entry[N].data + Entry[N-1].hash)`
- **Verification:** An external auditor can recalculate the hashes from Entry 0 to Entry N. If a rogue admin modifies or deletes Entry 5, the hash of Entry 5 changes, which invalidates the hash of Entry 6, and the chain breaks.
- **Storage:** Write these hash chains to a Write-Once-Read-Many (WORM) storage like AWS S3 with Object Lock enabled, making it literally impossible for even the root AWS account to delete the files before the retention period expires.
