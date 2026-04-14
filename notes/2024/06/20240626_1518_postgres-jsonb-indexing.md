---
title: Postgres JSONB indexing notes
slug: postgres-jsonb-indexing
tags: [postgres, sql, performance, backend]
description: When and how to index JSONB columns effectively
public: true
---

# Postgres JSONB indexing notes

Spent the afternoon tracking down a slow query that was doing full table scans on a JSONB column. Notes while it's fresh.

## GIN vs GiST for JSONB

For JSONB, GIN indexes are almost always what you want. GiST is better for geometric data and range types.

```sql
CREATE INDEX idx_events_metadata ON events USING GIN (metadata);
```

This handles `@>` (containment), `?` (key exists), and `?|`/`?&` operators.

## Expression indexes for specific paths

If you're always querying a specific nested key, an expression index is much smaller and faster:

```sql
CREATE INDEX idx_events_user_id ON events ((metadata->>'user_id'));
```

The double parentheses around the expression are required.

## Partial GIN for sparse data

If a key only exists on some rows, a partial index keeps things lean:

```sql
CREATE INDEX idx_events_flagged ON events USING GIN (metadata)
WHERE metadata ? 'flagged';
```

## Query that actually used the index after changes

```sql
SELECT * FROM events
WHERE metadata @> '{"status": "pending"}'
AND created_at > NOW() - INTERVAL '7 days';
```

Went from 4200ms to 38ms after adding the GIN index. The explain plan now shows `Bitmap Index Scan` instead of `Seq Scan`.

Reference: [Postgres JSONB indexing docs](https://www.postgresql.org/docs/current/datatype-json.html#JSON-INDEXING)
