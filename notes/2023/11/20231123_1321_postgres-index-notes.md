# Postgres index notes — partial and composite

Working through the slow query problem on the `events` table. Running `EXPLAIN ANALYZE` on the worst offenders revealed a seq scan on a table with 40M rows. Obvious in retrospect but we hadn't looked at it since the data grew past 5M.

## What I learned

**Partial indexes** are underused. If 90% of queries filter by `status = 'active'`, indexing just that subset is dramatically smaller and often faster:

```sql
CREATE INDEX events_active_user_idx
  ON events (user_id, created_at)
  WHERE status = 'active';
```

**Composite index column order matters** in a non-obvious way: the index supports filtering on a leading prefix of columns. So `(user_id, created_at)` helps `WHERE user_id = X` but not `WHERE created_at > Y` alone.

**Index-only scans** are possible when all needed columns are in the index. `INCLUDE` clause adds columns without them being part of the sort order:

```sql
CREATE INDEX events_user_created_idx
  ON events (user_id, created_at)
  INCLUDE (status, payload_size);
```

## Decisions made

Added two partial indexes targeting the reporting queries. Query time dropped from 4.2s to 80ms on the test dataset. Need to watch write performance — we insert ~15k rows/hour and the maintenance cost will matter.

Scheduled a re-check after two weeks of production traffic. See [Postgres documentation on indexes](https://www.postgresql.org/docs/current/indexes.html) for the full reference, and [Use The Index, Luke](https://use-the-index-luke.com/) for the best explanations of *why* the rules are what they are.
