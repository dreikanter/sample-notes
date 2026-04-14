---
title: SLIs, SLOs, and error budgets — working notes
slug: observability-sli-slo-notes
tags: [reliability, devops, reference]
---

# SLIs, SLOs, and error budgets — working notes

Writing these up as we set targets for the public API launch.

## Definitions

**SLI (Service Level Indicator):** A specific measurable metric. Examples:
- Request success rate (non-5xx responses / total responses)
- Latency (P95 response time)
- Availability (fraction of time the service is accessible)

**SLO (Service Level Objective):** A target for an SLI over a window. Examples:
- Success rate ≥ 99.5% over a rolling 30 days
- P95 latency ≤ 500ms, measured over 1 hour

**SLA (Service Level Agreement):** A contractual commitment. Usually more lenient than the SLO (internal buffer between what you target and what you promise).

**Error budget:** The amount of "room" you have to fail while still meeting the SLO. If SLO is 99.5% over 30 days:
- 30 days × 24 hours × 60 min = 43,200 minutes
- 0.5% of 43,200 = 216 minutes of allowable downtime

## What we set for v2

| SLI | SLO |
|-----|-----|
| Success rate | ≥ 99.5% over 30 days |
| P95 latency (non-auth) | ≤ 300ms |
| P95 latency (auth endpoints) | ≤ 500ms |
| Availability | ≥ 99.9% over 30 days |

## Error budget policy

When 50% of error budget consumed in a month:
- Freeze new features, focus on reliability

When 100% consumed:
- Incident review, root cause analysis required before new deployments

## Dashboard

The Grafana setup from [[20250405_1779]] tracks the SLIs. Added SLO lines as reference lines in each panel so the team can see the target at a glance.

Reference: [https://sre.google/sre-book/service-level-objectives/](https://sre.google/sre-book/service-level-objectives/)
