---
title: PostgreSQL indexing — practical patterns
slug: postgres-indexing
tags: [postgres, database, performance]
description: Indexing strategies I actually use, with EXPLAIN output notes.
---

# PostgreSQL indexing — practical patterns

Reference: https://www.postgresql.org/docs/current/indexes.html

## When a sequential scan beats an index

Postgres's query planner will often prefer a sequential scan for tables under ~1000 rows or when selectivity is low (i.e., the index would return a large fraction of rows). Don't add indexes reflexively — check with `EXPLAIN (ANALYZE, BUFFERS)`.

## Composite indexes and column order

The leftmost column in a composite index must appear in the WHERE clause for the index to be used. `(status, created_at)` helps queries filtering on `status` and sorting by `created_at`, but not queries that only filter on `created_at`.

```sql
CREATE INDEX idx_orders_status_created 
ON orders (status, created_at DESC);
```

## Partial indexes

Index only the rows you actually query. If 95% of your orders are 'completed' and you only ever query 'pending' ones:

```sql
CREATE INDEX idx_orders_pending 
ON orders (created_at)
WHERE status = 'pending';
```

Smaller, faster, and the planner will use it when conditions match.

## Expression indexes

```sql
CREATE INDEX idx_users_email_lower
ON users (lower(email));
```

Then query with `WHERE lower(email) = lower($1)`. Necessary when case-insensitive lookup is common.

## Index bloat

Postgres indexes accumulate dead tuples just like tables. Run `VACUUM ANALYZE` regularly, or rely on autovacuum. Check bloat with:

```sql
SELECT schemaname, tablename, pg_size_pretty(pg_total_relation_size(schemaname||'.'||tablename))
FROM pg_tables ORDER BY pg_total_relation_size(schemaname||'.'||tablename) DESC LIMIT 10;
```

## Covering indexes

Use `INCLUDE` to avoid heap fetches for index-only scans:

```sql
CREATE INDEX idx_orders_covering 
ON orders (user_id) INCLUDE (status, total);
```
