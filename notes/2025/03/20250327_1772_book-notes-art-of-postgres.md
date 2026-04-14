# Reading notes — The art of PostgreSQL

Started Dimitri Fontaine's book on PostgreSQL. It's a practitioner's text — assumes you know SQL and want to write it well. Organized around the idea that SQL is a programming language worth mastering rather than a necessary inconvenience.

## What's useful so far

**Window functions** — Chapter covers these well. I knew the syntax but didn't fully understand what the frame clause does. The frame determines which rows within the partition are included in the calculation:

```sql
SUM(amount) OVER (
    PARTITION BY customer_id
    ORDER BY order_date
    ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
)
```

That's a running total per customer. Change `ROWS` to `RANGE` and behavior changes when there are ties.

**Common table expressions (CTEs):** The section on recursive CTEs clicked better here than anywhere else I've seen it explained. The mental model of "seed rows, then iterate" made the syntax less mysterious.

**Indexing strategy:** Not just "add an index when queries are slow." Fontaine covers partial indexes, expression indexes, index-only scans, and how to read EXPLAIN ANALYZE output to verify an index is being used.

## What I'm skimping

The sections on data types and custom types. Probably worth returning to but not immediately relevant.

Book website: [https://theartofpostgresql.com](https://theartofpostgresql.com)
