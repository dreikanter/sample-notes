---
title: Monitoring stack setup — Grafana and Prometheus
slug: monitoring-setup
tags: [monitoring, devops, observability]
---

# Monitoring stack setup — Grafana and Prometheus

Set up a basic Prometheus + Grafana monitoring stack for a small service. Notes on the configuration that took time to figure out.

**Prometheus scrape config**

```yaml
# prometheus.yml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: 'app'
    static_configs:
      - targets: ['app:8080']
    metrics_path: '/metrics'

  - job_name: 'node'
    static_configs:
      - targets: ['node-exporter:9100']
```

**The alerting rule format**

```yaml
# alerts.yml
groups:
  - name: app
    rules:
      - alert: HighErrorRate
        expr: rate(http_requests_total{status=~"5.."}[5m]) > 0.05
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "High error rate on {{ $labels.job }}"
```

The `for: 5m` means the condition must be true for 5 continuous minutes before the alert fires. This prevents alerting on transient spikes.

**Grafana dashboard essentials**

The node-exporter-full dashboard (ID 1860 on grafana.com) gives you CPU, memory, disk, and network for any host running node-exporter. Import it rather than building from scratch.

For application metrics: use the rate() and histogram_quantile() functions.

```promql
# P95 request latency over 5 minutes
histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[5m]))
```

**What I learned**

Recording rules matter for complex queries. Pre-computing expensive PromQL expressions as new time series prevents slow dashboard loads.

Prometheus docs: https://prometheus.io/docs/practices/naming/
