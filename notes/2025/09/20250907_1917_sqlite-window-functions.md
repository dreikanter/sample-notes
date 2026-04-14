# SQLite window functions cheatsheet

Quick reference compiled after spending an afternoon debugging a reporting query. SQLite has supported window functions since version 3.25.0 (2018-09-15).

## Basic syntax

```sql
function_name(args) OVER (
  [PARTITION BY col1, col2]
  [ORDER BY col3]
  [frame_spec]
)
```

## Ranking functions

- `ROW_NUMBER()` — unique sequential integer, no ties
- `RANK()` — ties get same rank, gaps follow
- `DENSE_RANK()` — ties get same rank, no gaps
- `NTILE(n)` — divides rows into n buckets

## Running totals and moving averages

```sql
-- Running total
SUM(amount) OVER (ORDER BY date) AS running_total

-- 7-day moving average
AVG(value) OVER (
  ORDER BY date
  ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
) AS moving_avg
```

## LAG and LEAD

Useful for comparing current row to previous or next row without a self-join:

```sql
LAG(price, 1, 0) OVER (ORDER BY date) AS prev_price
LEAD(price, 1) OVER (PARTITION BY product_id ORDER BY date)
```

The third argument to LAG/LEAD is the default when no row exists.

## FIRST_VALUE / LAST_VALUE

`LAST_VALUE` requires explicit frame clause or it only sees the current row by default—a common gotcha:

```sql
LAST_VALUE(score) OVER (
  PARTITION BY student_id
  ORDER BY test_date
  ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
)
```

Full reference: [SQLite window functions documentation](https://www.sqlite.org/windowfunctions.html)
