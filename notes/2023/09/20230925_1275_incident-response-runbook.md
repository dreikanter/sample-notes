---
title: On-call runbook — high error rate incident
slug: incident-response-runbook
tags: [work, operations, runbook]
description: Runbook for the on-call rotation when error rates spike.
public: true
---

# On-call runbook — high error rate incident

This document is for the on-call rotation. Follow these steps when the error rate alert fires.

Alert: `ErrorRateHigh` — fires when 5xx responses exceed 2% of total requests over a 5-minute window.

## Step 1: assess scope (< 5 minutes)

Check the Grafana dashboard (link in #alerts channel). Determine:
- Which service(s) are throwing 5xx?
- Is it all endpoints or specific routes?
- What is the current rate vs. the baseline?
- When did it start?

## Step 2: check recent changes (< 5 minutes)

- Any deployments in the last hour? Check the deployment log in Slack (#deployments).
- Any config changes in AWS Parameter Store?
- Any third-party dependency incidents? Check https://www.githubstatus.com/ and your payment provider's status page.

## Step 3: classify the incident

**Deployment regression:** Roll back the deployment. Do not debug forward under load. Instructions for rollback: [internal link].

**Dependency failure:** Enable the circuit breaker for the failing dependency. Alert the dependency's owner or check their status page. Set a 15-minute check-in timer.

**Infrastructure issue:** Page the infrastructure on-call (separate rotation). Do not attempt to resolve infrastructure issues without infrastructure support.

**Unknown:** Open the incident bridge in Slack (#incident-bridge). Alert the engineering lead. Collect logs from the affected service(s).

## Step 4: communicate

Within 10 minutes of any customer-visible incident, post to #incidents: what is happening, who is on it, what the initial assessment is.

Reference: https://sre.google/sre-book/managing-incidents/
