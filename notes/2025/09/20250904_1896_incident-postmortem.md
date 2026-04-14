---
title: Incident postmortem — database connection exhaustion
slug: incident-postmortem
tags: [devops, incident, postmortem]
description: Postmortem from a production database connection exhaustion incident
public: true
---

# Incident postmortem — database connection exhaustion

**Incident summary**

On September 3, between 14:22 and 15:08, the application became intermittently unavailable. Error rate spiked to 40%. Root cause: database connection pool exhaustion caused by a long-running query holding connections open.

**Timeline**

14:22 — Alert fires on error rate
14:25 — On-call acknowledges; initial check shows healthy DB server (CPU 12%, disk OK)
14:31 — Identify that pool is exhausted via pg_stat_activity; 98 connections open of 100 max
14:38 — Identify a specific query running for 23 minutes on 40+ connections
14:41 — Kill the long-running queries manually; error rate drops immediately
14:48 — Incident resolved; monitoring for recurrence
15:08 — Root cause identified and fix deployed

**Root cause**

A new report endpoint was deployed in the previous release. The query it runs is unindexed and does a full table scan on a table with 8M rows. In testing it ran in 2s; in production with concurrent load it took 20+ minutes. Each request to the endpoint opened a connection and held it for the duration.

**Contributing factors**

- No connection timeout was configured (queries can hold connections indefinitely)
- The report endpoint was not load tested
- The connection pool limit (100) was set when the table was much smaller

**Actions**

- Immediate: add a statement timeout of 30s to prevent long queries from holding connections indefinitely
- Short term: add missing index on the report query (reduces from full scan to index scan)
- Medium term: move report queries to a read replica with a separate connection pool
- Process: load test new database-touching endpoints before release

Postmortem template from: https://sre.google/sre-book/postmortem-culture/
