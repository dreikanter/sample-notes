---
title: Observability — the three pillars
slug: observability-three-pillars
tags: [devops, observability, monitoring, reference]
description: Notes on logs, metrics, and traces and when to use each
---

# Observability — the three pillars

Working through the observability tooling at work. The three-pillar model is commonly cited; these are my working notes on what each actually does.

## Metrics

Numeric measurements over time. Low cardinality (few unique time series). Cheap to store, fast to query.

**Good for:** Alerting on thresholds, dashboards, capacity planning, SLO tracking.
**Bad for:** Debugging specific requests, understanding distributed system behavior.

Examples: `http_requests_total{method="GET", status="200"}`, `system_cpu_usage`, `order_payment_latency_p99`.

Tools: Prometheus + Grafana, Datadog, CloudWatch.

## Logs

Structured or unstructured events. Can have arbitrary fields (high cardinality fine). Expensive at scale.

**Good for:** Debugging specific events, audit trails, understanding exact error conditions.
**Bad for:** Aggregated trends without sampling, querying across high-volume services.

Best practice: structured JSON logs. Every log should have `timestamp`, `level`, `trace_id`, `service`, and a `message`. Anything that varies per-request goes in fields, not the message string.

```json
{"ts": "2024-04-10T14:22:01Z", "level": "error", "trace_id": "abc123",
 "service": "orders", "msg": "payment failed", "order_id": "789", "code": "insufficient_funds"}
```

## Traces

Distributed request tracing — following a request across service boundaries. A trace is a tree of spans, each representing a unit of work.

**Good for:** Finding bottlenecks in distributed systems, understanding request paths, debugging latency.
**Bad for:** Anything requiring aggregate statistics (use metrics).

Instrumentation adds small overhead. Sampling (capturing 1% or 10% of traces) is standard at high volume.

Tools: Jaeger, Zipkin, OpenTelemetry (vendor-neutral instrumentation).

## The combination

Metrics alert that something is wrong. Logs tell you what happened to specific requests. Traces show you where in the system it happened. None of the three covers the whole picture.

https://opentelemetry.io/docs/concepts/observability-primer/
