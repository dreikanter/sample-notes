# Understanding EXPLAIN ANALYZE in PostgreSQL

Query plans are the tool for understanding why a query is slow. Reference: [Use the Index, Luke](https://use-the-index-luke.com) for the best free resource on database performance.

## Basic usage

```sql
EXPLAIN ANALYZE SELECT * FROM orders WHERE customer_id = 123;
```

`EXPLAIN` shows the plan; `EXPLAIN ANALYZE` actually runs the query and shows real vs estimated times. Always use `ANALYZE` in development — the estimates alone can be misleading.

## Reading the output

```
Seq Scan on orders  (cost=0.00..4321.00 rows=1 width=64)
                    (actual time=0.043..43.211 rows=1 loops=1)
  Filter: (customer_id = 123)
  Rows Removed by Filter: 85432
```

**Key fields:**
- `cost=X..Y`: estimated startup cost..total cost (in arbitrary units)
- `rows=N`: estimated row count (compare to actual)
- `actual time=X..Y`: real startup..total milliseconds
- `loops=N`: how many times this node was executed

**Warning signs:**
- `Seq Scan` on a large table (no index being used)
- Large discrepancy between estimated and actual rows (stale statistics — run `ANALYZE table`)
- High `Rows Removed by Filter` with a Seq Scan (filtering a lot, no index)

## Common node types

- `Seq Scan`: reads the entire table — fine for small tables or large % of rows
- `Index Scan`: uses an index — better for selective queries
- `Index Only Scan`: can satisfy the query from the index alone — fastest
- `Bitmap Index Scan` + `Bitmap Heap Scan`: combination for medium-selectivity queries
- `Hash Join` / `Merge Join` / `Nested Loop Join`: different strategies for joining tables

## The critical check

```sql
EXPLAIN (ANALYZE, BUFFERS) SELECT ...;
```

Adding `BUFFERS` shows cache hit rates. High `shared hit` means data came from PostgreSQL's buffer cache (fast). High `shared read` means it came from disk (slow).

## Practical workflow

1. Identify the slow query (via `pg_stat_statements` or slow query log)
2. Run `EXPLAIN (ANALYZE, BUFFERS)` in a dev/staging environment
3. Look for `Seq Scan` on large tables, missing indexes, poor row estimates
4. Add index, run `ANALYZE`, verify plan improves
