---
title: SQLite quick reference
slug: sqlite-quick-ref
tags: [sqlite, databases, cheatsheet]
description: Commands and patterns I keep looking up
---

# SQLite quick reference

Running reference for SQLite CLI and common query patterns. Updated as I find gaps.

## CLI basics

```
sqlite3 mydb.db          # open or create
.tables                  # list tables
.schema tablename        # show CREATE statement
.mode column             # aligned output
.headers on              # show column names
.output file.csv         # redirect output
.separator ","           # for CSV
.import file.csv tname   # import CSV
.quit
```

## Useful pragmas

```sql
PRAGMA journal_mode = WAL;       -- better concurrent reads
PRAGMA foreign_keys = ON;        -- enforce FK constraints (off by default)
PRAGMA cache_size = -64000;      -- 64 MB page cache
PRAGMA synchronous = NORMAL;     -- safer than OFF, faster than FULL
```

## Window functions

```sql
SELECT name, score,
  RANK() OVER (PARTITION BY category ORDER BY score DESC) AS rnk,
  LAG(score, 1) OVER (ORDER BY date) AS prev_score
FROM results;
```

## JSON in SQLite (3.38+)

```sql
SELECT json_extract(data, '$.user.name') FROM events;
SELECT * FROM events WHERE json_each.value > 100
  JOIN json_each(data, '$.counts');
```

## FTS5 full-text search

```sql
CREATE VIRTUAL TABLE docs USING fts5(title, body);
INSERT INTO docs VALUES ('first', 'some body text');
SELECT * FROM docs WHERE docs MATCH 'body text';
```

Official docs: https://www.sqlite.org/lang.html

The WAL mode note from [[20250830_1012]] (Docker Compose) is relevant if running SQLite inside a container with a mounted volume — WAL + volume mounts can cause locking issues on some filesystems.
