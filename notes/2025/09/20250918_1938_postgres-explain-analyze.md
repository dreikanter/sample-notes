# Reading PostgreSQL EXPLAIN ANALYZE output

Always use `EXPLAIN (ANALYZE, BUFFERS, FORMAT TEXT)` rather than bare `EXPLAIN`. The extra options give timing and cache hit data.

## Basic structure

```
Seq Scan on orders  (cost=0.00..125.00 rows=5000 width=72)
                    (actual time=0.012..1.432 rows=5000 loops=1)
```

- `cost=start..total` — estimated cost units (not milliseconds)
- `rows` — estimated row count (compare to actual to spot planner inaccuracies)
- `actual time=first_row..last_row` — real milliseconds
- `loops` — how many times this node ran (multiply actual time by loops for total)

## Common node types

- **Seq Scan** — full table scan; bad on large tables unless you're fetching most of it
- **Index Scan** — uses index, follows heap pointers; good selectivity
- **Index Only Scan** — all needed columns in the index; no heap access
- **Bitmap Heap Scan** — fetches index pages first, then heap in block order; good for medium selectivity
- **Hash Join** / **Merge Join** / **Nested Loop** — join strategies; each has different trade-offs

## Things that indicate trouble

- `rows=1` estimated, `rows=50000` actual — planner is wrong, statistics stale → run `ANALYZE`
- High `loops` count in Nested Loop with a Seq Scan inside — missing index on join column
- `cost` much higher than similar queries — may be missing partial index

## Useful tools

`pg_stat_statements` tracks query frequencies and total time across a session, which is better than one-off EXPLAINs for finding expensive queries in production.

Reference: [PostgreSQL docs on EXPLAIN](https://www.postgresql.org/docs/current/sql-explain.html)

