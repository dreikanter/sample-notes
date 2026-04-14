---
title: OpenTelemetry primer
slug: opentelemetry-primer
tags: [observability, tracing, opentelemetry, engineering]
description: Core concepts and getting started with OpenTelemetry
---

# OpenTelemetry primer

The team is moving to standardized observability. Getting up to speed on OpenTelemetry.

## The three pillars

**Logs:** Discrete events with timestamps. What you're used to.

**Metrics:** Numeric measurements over time. Counters, gauges, histograms.

**Traces:** Requests across services as a directed acyclic graph. Each hop is a "span."

OpenTelemetry provides a unified SDK for all three so you don't need separate libraries.

## Core concepts

**Span:** A single unit of work. Has a start time, duration, status, and attributes.

**Trace:** A tree of spans representing a distributed operation. Connected by a `trace_id`.

**Context propagation:** How the trace ID follows a request across service boundaries. Usually via HTTP headers (W3C `traceparent`).

## Basic instrumentation (Node.js)

```javascript
const { NodeSDK } = require('@opentelemetry/sdk-node');
const { OTLPTraceExporter } = require('@opentelemetry/exporter-trace-otlp-http');

const sdk = new NodeSDK({
  traceExporter: new OTLPTraceExporter({
    url: 'http://otel-collector:4318/v1/traces',
  }),
});

sdk.start();
```

Auto-instrumentation handles Express, http, pg, and most other libraries automatically.

## Manual span creation

```javascript
const { trace } = require('@opentelemetry/api');
const tracer = trace.getTracer('my-service');

const span = tracer.startSpan('export-generation');
try {
  await generateExport(params);
  span.setStatus({ code: SpanStatusCode.OK });
} catch (err) {
  span.recordException(err);
  span.setStatus({ code: SpanStatusCode.ERROR });
} finally {
  span.end();
}
```

[OpenTelemetry documentation](https://opentelemetry.io/docs/)
