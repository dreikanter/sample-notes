---
title: Read replica design notes
slug: read-replica-design
tags: [database, postgres, architecture]
---

# Read replica design notes

Working through the architecture for routing reporting queries to the read replica. The basic concept is simple; the edge cases are not.

## Replication lag

With streaming replication, the replica will always be slightly behind the primary. Our reported acceptable lag: 5–10 seconds for the reporting use case. But we need to verify that assumption holds under load.

Test plan: simulate a write burst (5000 writes in 10 seconds), measure replica lag using `pg_stat_replication.write_lag` and `replay_lag` on the primary. If lag exceeds 30 seconds during burst, we need to reconsider acceptable thresholds or implement lag-aware routing.

## Routing approach

Considered three options:

1. **Application-level routing:** ORM configuration distinguishes reads vs writes at the model level. Clean, explicit. Requires touching every model class or using a base class.

2. **PgBouncer with separate pools:** One pool for writes, one for reads (pointing to replica). Application sends queries to the right pool based on context. Cleaner separation, but pool management overhead.

3. **Middleware/proxy:** Something like Crunchy Data's connection pooler or RDS Proxy. We don't run on RDS but a similar pattern. Most infrastructure overhead.

Recommendation: option 1 for now. We have a small codebase and can audit every query. Revisit if we add team members who don't know the distinction.

## Schema and data concerns

- Full-text search: replica should include all indexes from primary (happens automatically)
- Schema migrations: run against primary, propagate via replication. No migration should query the replica.
- Session variables: anything set via `SET LOCAL` on the primary doesn't replicate

Full reference: [Postgres streaming replication docs](https://www.postgresql.org/docs/current/warm-standby.html).
