---
title: SQLite as primary datastore – notes
slug: sqlite-as-primary-store
tags: [sqlite, databases, dev, architecture]
description: Research notes on using SQLite for a small personal web application
---

# SQLite as primary datastore – notes

From the backlog [[20251231_2025]]: "Experiment with SQLite as the primary datastore for a personal web app." Doing this now for the static site generator's upcoming CMS companion.

## Why SQLite for small apps

The conventional wisdom: SQLite is for development/testing; "real" apps use Postgres. This is wrong for apps with a certain profile:

- Single server, moderate load (thousands of requests per day, not millions)
- Read-heavy workload
- Simple queries, no complex joins at scale
- You want zero operational overhead

SQLite is a file. No separate process, no connection pooling, no configuration. The database is in the repository (or next to the binary). Backup is `cp`. Restore is `cp`.

## WAL mode

```sql
PRAGMA journal_mode=WAL;
```

Write-Ahead Logging allows one writer and multiple concurrent readers. Without this, any write locks the entire database. With WAL: reads never block writes, writes don't block reads. Enable this immediately.

```sql
PRAGMA synchronous=NORMAL;  -- safe with WAL, much faster than FULL
PRAGMA cache_size=-64000;   -- 64MB page cache
PRAGMA foreign_keys=ON;
```

## What SQLite can't do

- Multiple writers from multiple machines (not designed for distributed writes)
- Very large datasets (hundreds of GB — works but not optimal)
- Advanced Postgres features: JSONB with GIN indexes, `pg_stat_statements`, etc.

## Litestream for backup

Litestream (https://litestream.io) streams SQLite WAL changes to S3 in real time. This gives you continuous backup of a SQLite database with recovery point objective of a few seconds. Removes the last real operational concern.

Great overview: https://www.sqlite.org/whentouse.html
