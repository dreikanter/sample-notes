# Monitoring setup notes — Prometheus + Grafana

Setting up a proper monitoring stack for the service. These are working notes from the setup process.

## Prometheus configuration

```yaml
# prometheus.yml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

rule_files:
  - "alerts.yml"

scrape_configs:
  - job_name: 'app'
    static_configs:
      - targets: ['localhost:8080']
    metrics_path: /metrics
```

## Application instrumentation (Python)

```python
from prometheus_client import Counter, Histogram, Gauge, start_http_server

REQUEST_COUNT = Counter(
    'http_requests_total',
    'Total HTTP requests',
    ['method', 'endpoint', 'status']
)

REQUEST_LATENCY = Histogram(
    'http_request_duration_seconds',
    'HTTP request duration',
    ['method', 'endpoint'],
    buckets=[.005, .01, .025, .05, .1, .25, .5, 1, 2.5, 5]
)

# Usage
with REQUEST_LATENCY.labels(method='GET', endpoint='/api/users').time():
    result = do_the_work()
REQUEST_COUNT.labels(method='GET', endpoint='/api/users', status='200').inc()
```

## Alert rules

```yaml
# alerts.yml
groups:
  - name: app
    rules:
      - alert: HighErrorRate
        expr: |
          rate(http_requests_total{status=~"5.."}[5m])
          / rate(http_requests_total[5m]) > 0.05
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "Error rate above 5%"

      - alert: SlowRequests
        expr: |
          histogram_quantile(0.99,
            rate(http_request_duration_seconds_bucket[5m])
          ) > 2
        for: 5m
        annotations:
          summary: "p99 latency above 2s"
```

## Grafana dashboard essentials

Panels to always include:
- Request rate (RPS by status)
- Error rate (%)
- Latency distribution (p50, p95, p99)
- Active connections or queue depth
- System metrics (CPU, memory, disk)

Prometheus querying guide: https://prometheus.io/docs/prometheus/latest/querying/basics/
