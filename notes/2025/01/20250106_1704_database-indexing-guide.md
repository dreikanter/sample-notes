---
title: Database indexing — practical guide
slug: database-indexing-guide
tags: [postgresql, databases, performance, sql]
description: When and how to add indexes to improve query performance.
---

# Database indexing — practical guide

Notes from optimising several PostgreSQL databases. Reference: [Use the Index, Luke](https://use-the-index-luke.com) — the best free resource on this topic.

## When to add an index

- Column used in WHERE clauses frequently
- Column used in JOIN conditions
- Column used in ORDER BY when sorting large result sets
- Foreign keys (PostgreSQL doesn't auto-index them, unlike MySQL)

Don't index: rarely-queried columns, very small tables (Seq Scan is faster), columns with very low cardinality (e.g., a boolean with 50% true/50% false).

## Types of indexes in PostgreSQL

**B-tree** (default): best for equality and range queries, sorting. The right choice 90% of the time.

```sql
CREATE INDEX idx_orders_customer ON orders (customer_id);
CREATE INDEX idx_orders_created ON orders (created_at DESC);
```

**GIN (Generalized Inverted Index)**: full-text search, JSONB containment, array operators.

```sql
CREATE INDEX idx_articles_search ON articles USING GIN (to_tsvector('english', body));
CREATE INDEX idx_products_tags ON products USING GIN (tags);  -- tags is jsonb or array
```

**Partial index**: index only a subset of rows. Often dramatically smaller and faster.

```sql
-- Only index active users
CREATE INDEX idx_users_active_email ON users (email) WHERE status = 'active';
```

**Composite index**: covers multiple columns. Order matters — leading column must be in the query.

```sql
CREATE INDEX idx_orders_customer_status ON orders (customer_id, status);
-- Supports: WHERE customer_id = x
-- Supports: WHERE customer_id = x AND status = 'pending'
-- Does NOT support: WHERE status = 'pending' alone
```

## Monitoring index usage

```sql
-- Indexes with zero or low usage (candidates for removal)
SELECT indexrelname, idx_scan, idx_tup_read, idx_tup_fetch
FROM pg_stat_user_indexes
WHERE idx_scan < 50
ORDER BY idx_scan;

-- Table access patterns
SELECT relname, seq_scan, idx_scan
FROM pg_stat_user_tables
ORDER BY seq_scan DESC;
```

## Index maintenance

Indexes bloat over time with updates and deletes. `REINDEX` or `VACUUM FULL` rebuilds them. `pg_repack` can do this without locking. Monitor with `pg_stat_user_indexes`.

## Create indexes concurrently

```sql
CREATE INDEX CONCURRENTLY idx_large_table_col ON large_table (col);
```

Regular `CREATE INDEX` locks the table. `CONCURRENTLY` takes longer but doesn't block reads or writes.
