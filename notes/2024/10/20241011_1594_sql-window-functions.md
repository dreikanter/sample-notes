---
title: SQL window functions reference
slug: sql-window-functions-reference
tags: [sql, postgres, reference, analytics]
description: Window functions for analytical queries in SQL
---

# SQL window functions reference

Finally spent proper time understanding these. They're one of the most useful SQL features and I've been underusing them.

## What window functions are

Unlike GROUP BY (which collapses rows), window functions add computed values to each row by looking across a "window" of related rows. The original rows are preserved.

## OVER clause basics

```sql
SELECT
  order_id,
  amount,
  SUM(amount) OVER (PARTITION BY customer_id) as customer_total,
  RANK() OVER (PARTITION BY customer_id ORDER BY amount DESC) as rank_by_amount
FROM orders;
```

- `PARTITION BY` divides rows into groups (like GROUP BY but without collapsing)
- `ORDER BY` within OVER determines the row ordering within the window
- Without PARTITION BY: window is the entire result set

## Ranking functions

```sql
ROW_NUMBER()    -- unique sequential number, no ties
RANK()          -- ties get same rank, next rank skips (1,1,3)
DENSE_RANK()    -- ties get same rank, next rank doesn't skip (1,1,2)
NTILE(4)        -- divide into quartiles (or any N buckets)
```

## LAG and LEAD

Access values from preceding or following rows:

```sql
SELECT
  date,
  revenue,
  LAG(revenue, 1) OVER (ORDER BY date) as previous_day_revenue,
  revenue - LAG(revenue, 1) OVER (ORDER BY date) as daily_change
FROM daily_revenue;
```

## Running totals and moving averages

```sql
-- Running total
SUM(amount) OVER (ORDER BY date ROWS UNBOUNDED PRECEDING)

-- 7-day moving average
AVG(amount) OVER (
  ORDER BY date
  ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
)
```

[Mode Analytics SQL tutorial on window functions](https://mode.com/sql-tutorial/sql-window-functions/) is the best introduction I've found.
