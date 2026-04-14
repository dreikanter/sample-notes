---
title: PostgreSQL window functions quick reference
slug: postgres-window-functions
tags: [postgresql, sql, databases]
description: Practical examples of window functions for analytics queries.
---

# PostgreSQL window functions quick reference

Window functions perform calculations across rows related to the current row — like aggregate functions, but without collapsing rows into groups. See [PostgreSQL official docs](https://www.postgresql.org/docs/current/tutorial-window.html) for the full reference.

## Basic syntax

```sql
function_name(args) OVER (
  PARTITION BY column
  ORDER BY column
  ROWS/RANGE BETWEEN ... AND ...
)
```

## Common functions

**ROW_NUMBER()**: unique sequential number per partition

```sql
SELECT name, dept, salary,
  ROW_NUMBER() OVER (PARTITION BY dept ORDER BY salary DESC) AS rank
FROM employees;
```

**LAG / LEAD**: access previous/next row values

```sql
SELECT date, revenue,
  LAG(revenue, 1) OVER (ORDER BY date) AS prev_revenue,
  revenue - LAG(revenue, 1) OVER (ORDER BY date) AS delta
FROM sales;
```

**SUM with running total**:

```sql
SELECT date, amount,
  SUM(amount) OVER (ORDER BY date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total
FROM transactions;
```

**NTILE**: divide rows into N equal buckets

```sql
SELECT name, score,
  NTILE(4) OVER (ORDER BY score DESC) AS quartile
FROM results;
```

## Frame clauses

- `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` — running aggregate
- `ROWS BETWEEN 2 PRECEDING AND CURRENT ROW` — 3-row moving average
- `RANGE BETWEEN INTERVAL '7 days' PRECEDING AND CURRENT ROW` — 7-day rolling window (with date ORDER BY)

## Performance notes

Window functions run after WHERE, GROUP BY, and HAVING but before ORDER BY and LIMIT. They can be expensive on large tables without good index coverage on PARTITION BY and ORDER BY columns.
