# Read replica — production deployment notes

Deployed Wednesday, February 19. Eight months from identifying the problem to having a solution in production.

## What happened

The deployment itself was uneventful. Switched the ORM configuration to route the six reporting queries to the replica connection string. Monitored for two hours.

Initial replication lag: consistently under 500ms. Under the load test we ran last week it peaked at 2.1 seconds. Well within the 5–10 second threshold we defined as acceptable.

## Observed effects

Primary CPU load: dropped from 68% average to 47% average during the 10am–2pm reporting window. This was the main goal.

Reporting query times: essentially unchanged (the indexes were already the bottleneck for query planning, not database load). We expected this — the indexes were already doing the work. The replica change is about load distribution, not raw query speed.

No replication lag issues observed in the first 24 hours. The replica caught up after the initial sync in about 6 minutes, which matches our estimate.

## What to watch

- Replication lag under sustained write bursts (monthly billing run is in two weeks — will be the first real test)
- Any query that gets routed to the replica and expects primary-level freshness — we've audited for this but production has a way of surfacing edge cases
- Memory on the replica instance — currently at 68%, will grow as indexes are actively used

## Next steps

Document the routing decision for future engineers. The two-database configuration is non-obvious and someone will break it eventually if they don't know it exists.

Reference: [[20240129_1364]] for the design notes that preceded this. [Postgres replication monitoring](https://www.postgresql.org/docs/current/monitoring-stats.html#MONITORING-PG-STAT-REPLICATION-VIEW) for the queries I'm using to watch lag.
