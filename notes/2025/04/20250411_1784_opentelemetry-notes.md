# OpenTelemetry — getting started notes

Working through OpenTelemetry as part of the observability improvements for v2. Notes on concepts and setup.

## Core signals

**Traces:** Distributed request tracking. A trace is a collection of spans — each span represents a unit of work in a service. Spans have a parent-child relationship forming a tree. Critical for diagnosing latency in microservices.

**Metrics:** Numerical measurements over time. OTel provides an SDK for creating counters, gauges, and histograms. Can export to Prometheus, Grafana Cloud, etc.

**Logs:** Structured log events. OTel is adding log support but it's less mature than traces and metrics.

## Python setup (FastAPI example)

```python
from opentelemetry import trace
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor
from opentelemetry.exporter.otlp.proto.grpc.trace_exporter import OTLPSpanExporter
from opentelemetry.instrumentation.fastapi import FastAPIInstrumentor

# Setup
provider = TracerProvider()
exporter = OTLPSpanExporter(endpoint="http://otel-collector:4317")
provider.add_span_processor(BatchSpanProcessor(exporter))
trace.set_tracer_provider(provider)

# Auto-instrument FastAPI
FastAPIInstrumentor.instrument_app(app)

# Manual spans
tracer = trace.get_tracer(__name__)

with tracer.start_as_current_span("my-operation") as span:
    span.set_attribute("user.id", user_id)
    span.set_attribute("operation.type", "lookup")
    result = do_something()
```

## Collector configuration

The OTel Collector receives telemetry, processes it, and exports to backends. Basic config:

```yaml
receivers:
  otlp:
    protocols:
      grpc:
        endpoint: 0.0.0.0:4317

exporters:
  prometheus:
    endpoint: "0.0.0.0:8889"
  jaeger:
    endpoint: jaeger:14250

service:
  pipelines:
    traces:
      receivers: [otlp]
      exporters: [jaeger]
    metrics:
      receivers: [otlp]
      exporters: [prometheus]
```

Reference: [https://opentelemetry.io/docs/](https://opentelemetry.io/docs/)
