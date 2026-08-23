# Round 5 — LLD & Light System Design
**Interview:** Developer Productivity / Tooling Company

---

## LLD Problem — Extensible Code Formatter / Linter

> "Design the core architecture for a code linter (like ESLint). It needs to be extensible so third-party developers can write their own rules. You don't have to write the parser, assume you are handed an AST (Abstract Syntax Tree)."

**Classes / Interfaces:**
```
interface ASTNode {
  type: string; // e.g., 'FunctionDeclaration', 'VariableDeclaration'
  children: ASTNode[];
  // ... line/column metadata
}

interface LinterRule {
  ruleId: string;
  // Visitor pattern: returns a map of node types to callback functions
  create(context: RuleContext): Record<string, (node: ASTNode) => void>;
}

class RuleContext {
  report(node: ASTNode, message: string): void;
}

class Linter {
  rules: LinterRule[];
  
  registerRule(rule: LinterRule): void;
  lint(ast: ASTNode): LintError[];
}
```

**Design questions I'll ask:**
1. "How do you traverse the tree so that rules aren't iterating over the tree 50 different times for 50 different rules?"
   *Expected:* The Visitor Pattern. The `Linter` core does exactly ONE depth-first traversal of the AST. At each node, it checks if any registered rules care about `node.type`, and invokes their callbacks. This makes adding rules O(1) in terms of traversal overhead.

---

## Light System Design — Distributed CI/CD Pipeline

> "Design a CI/CD build runner system (like GitHub Actions). Users submit a `.yml` file. We need to spin up a fresh environment, run their arbitrary code safely, stream the logs back to the UI in real time, and tear it down."

**Architecture:**
- **Control Plane:** API server receives the webhook trigger. Parses the `.yml`. Enqueues a build job in a distributed queue (e.g., Redis/RabbitMQ).
- **Worker Pool:** A fleet of Auto-Scaling VMs. They pop jobs from the queue.
- **Execution:** The worker pulls a base Docker image, injects the user's repository, and runs the commands.
- **Log Streaming:** The Docker container's `stdout` is piped to a local agent on the worker, which flushes chunks over WebSockets (or gRPC) to a Log Aggregator (e.g., Redis Streams or Kafka), which the UI consumes.

**Key constraints to discuss:**
- **Security:** You are running untrusted code. A basic Docker container on a shared host is NOT enough. You need ephemeral VMs (AWS Firecracker/EC2) per tenant.
- **Log ordering:** Logs must appear in the exact order they were emitted.
