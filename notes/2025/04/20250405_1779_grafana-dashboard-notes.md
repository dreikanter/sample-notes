# Grafana dashboard setup notes

Notes from setting up the API monitoring dashboard after the v2 beta launch.

## Data source configuration

Using Prometheus as the primary data source. Connected via the standard Prometheus data source plugin. Scrape interval is 15s; evaluation interval is 15s. Setting these too low causes storage issues; too high loses resolution.

## Panels I set up

**Request rate:** Line chart, `rate(http_requests_total[5m])`, labeled by endpoint. Useful for spotting traffic spikes and seeing which endpoints are busiest.

**Error rate:** Same metric filtered to `{status=~"5.."}`. Stack this with the success rate in the same panel for visual proportion.

**P95 latency:** `histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[5m]))` — surfaces slow requests that averages would hide.

**Active users:** Counter via a custom gauge metric tracking active sessions. Simple number panel with a threshold color (green below 100, yellow 100-500, red above).

**Database pool utilization:** Custom metric from the app. Helps correlate slowdowns with DB pressure.

## Dashboard variables

Used template variables to make the dashboard work across environments:

```
$env = dev | staging | production
```

Reference the variable in queries as `{environment="$env"}`.

## Alerting

Set up two alerts in Grafana:
- Error rate > 5% for 5 minutes → page
- P95 latency > 2s for 10 minutes → page

Alerts route to PagerDuty via a webhook receiver. Silence rules configured for known maintenance windows.

Grafana docs: [https://grafana.com/docs/grafana/latest/](https://grafana.com/docs/grafana/latest/)
