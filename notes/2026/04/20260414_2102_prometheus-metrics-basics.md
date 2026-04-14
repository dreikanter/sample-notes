# Prometheus metrics basics

Notes from adding observability to the API gateway (see [[20260312_2072]]).

## Metric types

**Counter**: monotonically increasing. Use for totals: requests served, errors, bytes processed. Never decreases (except on restart).

```go
requestsTotal := prometheus.NewCounterVec(
    prometheus.CounterOpts{
        Name: "http_requests_total",
        Help: "Total HTTP requests processed",
    },
    []string{"method", "path", "status"},
)
```

**Gauge**: can go up or down. Use for current state: active connections, queue length, temperature.

**Histogram**: samples observations and counts them in configurable buckets. Use for latency distributions.

```go
requestDuration := prometheus.NewHistogramVec(
    prometheus.HistogramOpts{
        Name:    "http_request_duration_seconds",
        Help:    "HTTP request latency distribution",
        Buckets: prometheus.DefBuckets,  // .005, .01, .025, .05, .1, .25, .5, 1, 2.5, 5, 10
    },
    []string{"method", "path"},
)
```

**Summary**: similar to histogram but pre-computes quantiles. Less flexible; prefer histograms.

## Go instrumentation

```go
// Register with default registry
prometheus.MustRegister(requestsTotal, requestDuration)

// Record in handler
func handleRequest(w http.ResponseWriter, r *http.Request) {
    start := time.Now()
    // ... handle request ...
    duration := time.Since(start).Seconds()

    requestsTotal.WithLabelValues(r.Method, r.URL.Path, "200").Inc()
    requestDuration.WithLabelValues(r.Method, r.URL.Path).Observe(duration)
}

// Expose metrics endpoint
http.Handle("/metrics", promhttp.Handler())
```

## Key labels warning

Labels multiply time series. `method * path * status` can explode if path isn't normalized (every unique user ID in the path = separate series). Normalize paths before labeling.

## Useful PromQL

```
rate(http_requests_total[5m])                    # request rate
histogram_quantile(0.99, rate(http_request_duration_seconds_bucket[5m]))  # p99 latency
```

Reference: https://prometheus.io/docs/practices/naming/
Go client: https://github.com/prometheus/client_golang
