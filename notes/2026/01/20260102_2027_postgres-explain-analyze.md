# Postgres EXPLAIN ANALYZE notes

Working notes from reading through slow query logs at work. This is the mental model I'm building for reading query plans.

## Basic usage

```sql
EXPLAIN ANALYZE SELECT * FROM orders WHERE user_id = 42;
```

`EXPLAIN` shows the plan without executing. `EXPLAIN ANALYZE` executes and shows actual timings — always use for real debugging.

## Reading the output

The plan is a tree. Each node is an operation. Read bottom-up: the innermost (most indented) nodes execute first.

Key numbers:
- `cost=0.00..8.29` — startup cost .. total cost (planner's estimate, in arbitrary units)
- `rows=1` — estimated row count
- `actual time=0.015..0.017` — actual ms
- `rows=1 loops=1` — actual rows, loop count

## Warning signs

- **Seq Scan on a large table** — may need an index
- **High row estimate vs actual** — statistics may be stale; run `ANALYZE tablename`
- **Hash Join on huge tables** — might be okay, might need tuning `work_mem`
- **Nested Loop with many rows** — can be O(n²), check if index is being used

## Useful options

```sql
EXPLAIN (ANALYZE, BUFFERS, FORMAT JSON) SELECT ...;
```

`BUFFERS` shows cache hits vs disk reads. `FORMAT JSON` is parseable and works with visual tools like https://explain.dalibo.com — paste the JSON output and get a visual plan.

## The `pg_stat_statements` extension

```sql
CREATE EXTENSION pg_stat_statements;
SELECT query, mean_exec_time, calls FROM pg_stat_statements ORDER BY mean_exec_time DESC LIMIT 10;
```

This is the fastest way to find what's actually slow in production.

Reference: https://www.postgresql.org/docs/current/using-explain.html
