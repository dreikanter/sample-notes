---
title: On-call runbook — first draft
slug: on-call-runbook
tags: [work, operations, runbook]
---

# On-call runbook — first draft

Finally writing this (promised since November, see [[20231126_1325]]). Draft, needs team review before it's the real thing.

## Scope

This covers the event pipeline, the main API service, and the database layer. Auth service has its own runbook maintained by the team that owns it.

## Escalation

PagerDuty is primary alert channel. Slack #incidents for communication. The on-call engineer handles first response. If not resolved in 30 minutes or if severity is high, page the secondary (currently rotating weekly between Devlin and Marcus).

Never page someone without acknowledging the alert yourself first. "Escalating because I'm stuck" is valid. "Escalating because I didn't look at it" is not.

## Event pipeline failures

**Symptom:** Kafka consumer lag > 10,000 messages and growing.  
**First step:** Check consumer process status — `systemctl status pipeline-consumer` on the worker.  
**Common cause:** Database insert failure blocking the processing loop.  
**Resolution:** Check Postgres primary health. If Postgres is healthy, look for duplicate event_id constraint violations in the consumer log.

**Symptom:** Redis counters diverging from Postgres.  
**Action:** Do not manually correct Redis. Restart the consumer — it reconciles on startup (see [[20231212_1337]] for architecture detail).

## Database issues

**Replica lag > 30 seconds sustained:**  
Check `pg_stat_replication` on the primary. If `replay_lag` is growing and the replica is healthy, look for long-running transactions on the primary blocking replication.

**Connection pool exhaustion:**  
PgBouncer shows no available connections. First: check for long-running queries (>60s) that are holding connections. Kill them if they're stuck. Then page Devlin — pool sizing may need adjustment.

Reference: [Charity Majors on on-call culture](https://charity.wtf/category/observability/) for the philosophy behind how we've set this up.
