# Reading notes — observability and monitoring

Been working through Charity Majors' blog after mentioning her in the on-call runbook (see [[20240302_1380]]). These are the ideas that changed how I think about monitoring.

## The three pillars

Traditionally described as metrics, logs, and traces. The framework is useful but Majors' critique is valuable: three independent pillars don't give you *observability* — they give you three separate dashboards to check when something goes wrong. Observability is about being able to ask arbitrary questions of your system state without knowing in advance what you'll need to ask.

## High cardinality

The key insight from Honeycomb's approach: metrics are aggregates and aggregates hide the individual events that matter during debugging. If p99 latency spikes, you want to know *which* requests are slow — by user, by endpoint, by feature flag, by server, by anything. Aggregated metrics can't answer that.

High-cardinality data (millions of unique values) needs different storage and query patterns than metrics (thousands of values, heavily aggregated). This is why purpose-built tools like Honeycomb exist.

## Practical takeaway for our system

Our current monitoring has good metrics (latency percentiles, error rates, throughput) but essentially no tracing. When a request fails, I can see that it failed but can't easily trace what path it took through the system. For a service with multiple downstream calls (database, Redis, Kafka), this is a real gap.

Adding OpenTelemetry traces is in the Q2 backlog. Start with the three most complex endpoints.

## Reference materials

- [Honeycomb's observability engineering guide](https://www.honeycomb.io/resources/oreilly-observability-engineering-ebook) — free ebook
- [Charity Majors' blog](https://charity.wtf/) — the most practical writing on production engineering I've found
- [OpenTelemetry documentation](https://opentelemetry.io/docs/) for the implementation spec
