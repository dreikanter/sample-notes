# Conference notes — distributed systems patterns

Notes from a recorded conference talk I watched this weekend. [QCon SF 2024 talks](https://www.infoq.com/qcon-sf-2024/) are available on InfoQ.

## Talk: "Patterns for resilience at scale"

### The Fallacies of Distributed Computing

Speaker opened with the eight [classic fallacies](https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing): the network is reliable, latency is zero, bandwidth is infinite, etc. Still relevant. The point was that most outages trace to an engineer who forgot one of these.

### Circuit breakers

A circuit breaker prevents a cascade failure by short-circuiting calls to a failing downstream service after a threshold of errors. States: closed (calls pass through), open (calls fail fast), half-open (test calls to see if recovery happened).

The speaker recommended configuring per-dependency rather than globally, and being explicit about what constitutes a failure vs. a slow response.

### Bulkheads

Partition resources (threads, connections) so that saturation in one path doesn't starve others. Named after ship hull compartments. In practice: separate thread pools per dependency, separate connection pools per database.

### Idempotency as a design principle

Make operations safe to retry. Assign client-generated idempotency keys on writes. The server stores the key and returns the same response if it sees a duplicate—client can retry without fear.

### Saga pattern for long transactions

Instead of distributed transactions (two-phase commit is fragile), model as a series of local transactions with compensating transactions to undo on failure. Requires careful ordering and explicit failure handling.

Good talk. About 45 minutes. Worth the time.

