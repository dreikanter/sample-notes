# PostgreSQL window functions — working notes

These come up regularly enough that I keep rewriting them from scratch. Pinning the patterns I actually use.

Reference: [PostgreSQL docs — window functions](https://www.postgresql.org/docs/current/tutorial-window.html)

---

## Basic syntax

```sql
SELECT
  column,
  function() OVER (
    PARTITION BY partition_col
    ORDER BY order_col
    ROWS BETWEEN frame_start AND frame_end
  )
FROM table;
```

`PARTITION BY` is optional. Without it, the whole result set is one window.

---

## Functions I use most

### Ranking

```sql
ROW_NUMBER() OVER (PARTITION BY dept ORDER BY salary DESC)
-- Unique rank, no gaps, no ties

RANK() OVER (...)
-- Ties get same rank, next rank skips (1,2,2,4)

DENSE_RANK() OVER (...)
-- Ties get same rank, no gap (1,2,2,3)
```

### Offsets

```sql
LAG(price, 1) OVER (PARTITION BY ticker ORDER BY date)
-- Previous row's value; NULL for first row

LEAD(price, 1) OVER (...)
-- Next row's value

FIRST_VALUE(col) OVER (...)
LAST_VALUE(col) OVER (ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING)
-- LAST_VALUE needs explicit frame or you get current row as last
```

### Running aggregates

```sql
SUM(amount) OVER (PARTITION BY account ORDER BY date ROWS UNBOUNDED PRECEDING)
-- Running total per account

AVG(value) OVER (ORDER BY date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW)
-- 7-day rolling average
```

---

## Get the top N per group

```sql
SELECT * FROM (
  SELECT
    *,
    ROW_NUMBER() OVER (PARTITION BY category ORDER BY score DESC) AS rn
  FROM products
) sub
WHERE rn <= 3;
```

---

## FILTER inside window

Not all databases support this; Postgres does from 9.4:

```sql
COUNT(*) FILTER (WHERE status = 'active') OVER (PARTITION BY region)
```

---

## Pitfalls

- `WHERE` cannot reference window function results — use a subquery or CTE
- `LAST_VALUE` with the default frame (`RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`) gives the current row, not the partition's last row — the frame spec above fixes it
- Window functions run after `WHERE` and `GROUP BY` but before `ORDER BY` and `LIMIT`

Last used these heavily when building the cohort retention query in the analytics work around [[20250407_1082]].
