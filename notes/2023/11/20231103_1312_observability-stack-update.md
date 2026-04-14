# Observability stack update — adding distributed tracing

The Prometheus/Grafana setup from September [[20230922_1272]] gives us metrics and dashboards. What's missing: distributed traces. When a slow request crosses three services, we need to see where the time went.

Adding OpenTelemetry.

Reference: https://opentelemetry.io/docs/

## Architecture decision

Using OpenTelemetry's SDK for instrumentation (language-agnostic), exporting to Jaeger for trace storage and visualization. Alternative was Zipkin — similar feature set, chose Jaeger because we're already using it in a couple of internal tools.

## Node.js instrumentation

```javascript
const { NodeSDK } = require('@opentelemetry/sdk-node');
const { OTLPTraceExporter } = require('@opentelemetry/exporter-trace-otlp-http');
const { getNodeAutoInstrumentations } = require('@opentelemetry/auto-instrumentations-node');

const sdk = new NodeSDK({
  traceExporter: new OTLPTraceExporter({
    url: 'http://jaeger:4318/v1/traces',
  }),
  instrumentations: [getNodeAutoInstrumentations()],
});

sdk.start();
```

Auto-instrumentation covers Express, HTTP, fetch, and several database clients automatically — no manual span creation needed for most cases.

## Custom spans

For business logic that isn't covered by auto-instrumentation:

```javascript
const { trace } = require('@opentelemetry/api');
const tracer = trace.getTracer('my-service');

async function processOrder(orderId) {
  const span = tracer.startSpan('process-order', {
    attributes: { 'order.id': orderId }
  });

  try {
    const result = await doWork(orderId);
    span.setStatus({ code: SpanStatusCode.OK });
    return result;
  } catch (err) {
    span.recordException(err);
    span.setStatus({ code: SpanStatusCode.ERROR });
    throw err;
  } finally {
    span.end();
  }
}
```

## First results

Deployed to the notification service on the EKS pilot cluster yesterday. First traces in Jaeger showed a database query we thought was taking ~15ms actually taking ~180ms due to connection pool contention. Not visible in metrics. Exactly what tracing is for.
