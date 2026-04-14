---
title: Observability basics — the three pillars
slug: observability-basics
tags: [observability, devops, monitoring, backend]
public: true
---

# Observability basics — the three pillars

Notes on the three-pillar model (logs, metrics, traces). Good reference: [Honeycomb's observability guide](https://www.honeycomb.io/what-is-observability).

## Logs

Structured records of events. The shift from unstructured text logs to structured (JSON) logs makes them queryable:

```json
{
  "timestamp": "2024-12-11T10:23:14Z",
  "level": "error",
  "message": "Failed to process payment",
  "user_id": "u_12345",
  "order_id": "o_67890",
  "error_code": "CARD_DECLINED",
  "duration_ms": 234
}
```

**Best practices**: log at the right level (debug/info/warn/error); include correlation IDs to trace a request across services; never log sensitive data.

## Metrics

Numeric measurements over time. Typically aggregated (counters, gauges, histograms). Low storage cost, efficient to query, not good for debugging specific incidents.

Common types:
- **Counter**: monotonically increasing (request count, error count)
- **Gauge**: point-in-time value (memory usage, active connections)
- **Histogram**: distribution of values (request duration, payload size)

**Key metrics to always have**: request rate, error rate, latency (p50/p95/p99), resource saturation (CPU, memory).

The [RED method](https://grafana.com/blog/2018/08/02/the-red-method-how-to-instrument-your-services/) (Rate, Errors, Duration) is a clean minimal set for service monitoring.

## Traces

A trace follows a request across multiple services. Each step is a "span" with timing, metadata, and parent/child relationships. Traces are invaluable for understanding latency and diagnosing failures in distributed systems.

[OpenTelemetry](https://opentelemetry.io) is the standard for instrumenting traces (and increasingly logs and metrics).

## The practical reality

Most teams need all three but implement them poorly. The most common failure: logs that are hard to query (no structure, no correlation IDs), metrics that don't cover the application layer (only infrastructure), and no tracing at all. Start with structured logs and key application metrics — they give 80% of the value with 20% of the effort.
