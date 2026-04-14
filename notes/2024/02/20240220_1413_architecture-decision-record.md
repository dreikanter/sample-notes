---
title: ADR — read replica for reporting queries
slug: architecture-decision-record
tags: [work, adr, database, architecture]
---

# ADR — read replica for reporting queries

**Date:** 2024-02-20  
**Status:** Accepted  
**Deciders:** Engineering team (consensus at offsite, February 7)

## Context

The `events` table on our primary PostgreSQL instance receives approximately 15,000 inserts per hour. Reporting queries (aggregations over time windows, trend calculations) were competing with write traffic, causing p99 latency on the primary to reach 4.2 seconds during peak periods.

We added partial indexes in November 2023 (see [[20231123_1321]]) which reduced query time to 80ms, but did not reduce load on the primary.

## Decision

Route all reporting queries to a streaming read replica of the primary. Keep all write traffic and latency-sensitive reads on the primary.

## Rationale

- Read replicas are well-understood infrastructure; no new technology
- Separates read and write concerns cleanly
- Replication lag (< 3 seconds under normal load) is acceptable for reporting use cases
- Lower risk than alternatives considered (ClickHouse, materialized views)

## Alternatives considered

**Materialized views + scheduled refresh:** Would serve pre-computed results but requires knowing in advance which aggregations are needed. Limits flexibility.

**ClickHouse:** Superior for analytics at scale. Operational complexity and migration effort not justified at our current scale. Revisit if read replica proves insufficient.

## Consequences

Positive: Primary CPU load reduced from 68% to 47% during peak window.

Negative: Engineers must be aware of the two-connection configuration and route queries appropriately. Risk of incorrect routing (sending time-sensitive reads to replica) is mitigated by documentation and code review.

Reference: [Postgres streaming replication docs](https://www.postgresql.org/docs/current/warm-standby.html). ADR format from [Michael Nygard](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions).
