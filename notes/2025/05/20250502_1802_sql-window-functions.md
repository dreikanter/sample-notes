# SQL window functions — working reference

Window functions operate on a set of rows related to the current row, without collapsing them into groups like GROUP BY. Clarified significantly after reading the Fontaine book (see [[20250327_1772]]).

## Syntax

```sql
function_name() OVER (
    [PARTITION BY column, ...]
    [ORDER BY column, ...]
    [frame_clause]
)
```

## Ranking functions

```sql
SELECT
    name,
    department,
    salary,
    RANK()       OVER (PARTITION BY department ORDER BY salary DESC) AS rank,
    DENSE_RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS dense_rank,
    ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS row_num,
    PERCENT_RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS pct
FROM employees;
```

`RANK` has gaps after ties; `DENSE_RANK` does not; `ROW_NUMBER` is always unique.

## Aggregate as window function

```sql
SELECT
    order_id,
    amount,
    customer_id,
    SUM(amount)  OVER (PARTITION BY customer_id) AS customer_total,
    AVG(amount)  OVER (PARTITION BY customer_id) AS customer_avg,
    COUNT(*)     OVER (PARTITION BY customer_id) AS order_count
FROM orders;
```

## Running totals (frame clause)

```sql
SELECT
    date,
    revenue,
    SUM(revenue) OVER (
        ORDER BY date
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS running_total,
    AVG(revenue) OVER (
        ORDER BY date
        ROWS BETWEEN 6 PRECEDING AND CURRENT ROW  -- 7-day moving average
    ) AS moving_avg_7d
FROM daily_revenue;
```

## LAG and LEAD

```sql
SELECT
    date,
    revenue,
    LAG(revenue, 1) OVER (ORDER BY date)  AS prev_day,
    LEAD(revenue, 1) OVER (ORDER BY date) AS next_day,
    revenue - LAG(revenue, 1) OVER (ORDER BY date) AS day_over_day
FROM daily_revenue;
```

## FIRST_VALUE, LAST_VALUE, NTH_VALUE

```sql
FIRST_VALUE(salary) OVER (PARTITION BY dept ORDER BY salary DESC)
LAST_VALUE(salary)  OVER (PARTITION BY dept ORDER BY salary DESC ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING)
```

Note: `LAST_VALUE` needs explicit frame or you only get the current row.

Postgres docs: [https://www.postgresql.org/docs/current/tutorial-window.html](https://www.postgresql.org/docs/current/tutorial-window.html)
