# PostgreSQL window functions

## Basic structure

```sql
function_name() OVER (
  PARTITION BY col1, col2
  ORDER BY col3
  ROWS/RANGE frame_clause
)
```

## Ranking functions

```sql
SELECT
  name,
  department,
  salary,
  ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS row_num,
  RANK()       OVER (PARTITION BY department ORDER BY salary DESC) AS rank,
  DENSE_RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS dense_rank
FROM employees;
-- RANK skips numbers after ties; DENSE_RANK doesn't
```

## Lag and lead

```sql
SELECT
  date,
  revenue,
  LAG(revenue, 1) OVER (ORDER BY date) AS prev_revenue,
  revenue - LAG(revenue, 1) OVER (ORDER BY date) AS delta
FROM daily_revenue;
```

## Running totals

```sql
SELECT
  date,
  amount,
  SUM(amount) OVER (ORDER BY date ROWS UNBOUNDED PRECEDING) AS running_total
FROM transactions;
```

## nth_value and first/last

```sql
SELECT
  id,
  value,
  FIRST_VALUE(value) OVER w AS first_in_group,
  LAST_VALUE(value)  OVER w AS last_in_group
FROM data
WINDOW w AS (PARTITION BY group_id ORDER BY id
             ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING);
```

## Named windows

When using the same window spec multiple times, define it once:

```sql
SELECT name, salary,
  AVG(salary) OVER dept_window,
  MAX(salary) OVER dept_window
FROM employees
WINDOW dept_window AS (PARTITION BY department);
```

Docs: https://www.postgresql.org/docs/current/tutorial-window.html
