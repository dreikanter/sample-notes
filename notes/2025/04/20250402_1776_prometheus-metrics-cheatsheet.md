# Prometheus metrics cheatsheet

Reference for Prometheus metric types, naming conventions, and PromQL queries I use.

## Metric types

**Counter:** Monotonically increasing. Use for things counted over time (requests, errors). Never decreases.

```python
from prometheus_client import Counter
requests_total = Counter('http_requests_total', 'Total HTTP requests', ['method', 'endpoint', 'status'])
requests_total.labels(method='GET', endpoint='/api/users', status='200').inc()
```

**Gauge:** Can go up or down. Use for current state (queue size, active connections, memory).

```python
from prometheus_client import Gauge
queue_size = Gauge('job_queue_size', 'Number of jobs in queue')
queue_size.set(42)
queue_size.inc()
queue_size.dec()
```

**Histogram:** Samples observations, calculates buckets. Good for latency and request size.

```python
from prometheus_client import Histogram
response_time = Histogram('http_response_seconds', 'Response time', buckets=[0.01, 0.05, 0.1, 0.5, 1.0, 5.0])
with response_time.time():
    handle_request()
```

## Naming conventions

```
# Format: library_name_unit_suffix
# Examples:
http_requests_total
http_request_duration_seconds
process_memory_bytes
job_queue_length
```

## Common PromQL queries

```
# Request rate per second over last 5 minutes
rate(http_requests_total[5m])

# Error rate
rate(http_requests_total{status=~"5.."}[5m])

# 95th percentile latency
histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[5m]))

# CPU usage
100 - (avg(rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
```

Prometheus docs: [https://prometheus.io/docs/practices/naming/](https://prometheus.io/docs/practices/naming/)
