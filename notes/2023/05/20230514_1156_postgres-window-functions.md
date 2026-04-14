---
title: PostgreSQL window functions — practical examples
slug: postgres-window-functions
tags: [postgresql, sql, reference]
---

# PostgreSQL window functions — practical examples

Window functions perform calculations across rows related to the current row, without collapsing rows like GROUP BY does.

**Syntax**

```sql
function_name() OVER (
  PARTITION BY column
  ORDER BY column
  ROWS/RANGE BETWEEN ... AND ...
)
```

**Running total**

```sql
SELECT
  date,
  amount,
  SUM(amount) OVER (ORDER BY date) AS running_total
FROM transactions;
```

**Rank within partition**

```sql
SELECT
  department,
  name,
  salary,
  RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS dept_rank
FROM employees;
```

`RANK()` leaves gaps; `DENSE_RANK()` does not; `ROW_NUMBER()` always unique.

**Previous/next row values**

```sql
SELECT
  date,
  value,
  LAG(value, 1) OVER (ORDER BY date) AS prev_value,
  LEAD(value, 1) OVER (ORDER BY date) AS next_value
FROM metrics;
```

**Percentile within partition**

```sql
SELECT
  category,
  amount,
  NTILE(4) OVER (PARTITION BY category ORDER BY amount) AS quartile
FROM sales;
```

**Frame specification**

```sql
-- 7-day moving average
AVG(value) OVER (
  ORDER BY date
  ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
)
```

Docs: [postgresql.org/docs/current/tutorial-window.html](https://www.postgresql.org/docs/current/tutorial-window.html)
