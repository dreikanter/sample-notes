---
title: Migrating from SQLite to PostgreSQL
slug: migrating-to-postgresql
tags: [postgresql, sqlite, database, migration]
description: Practical notes from a SQLite to PostgreSQL migration
---

# Migrating from SQLite to PostgreSQL

Did this migration for the side project — went from SQLite in development to PostgreSQL in production. Notes on the gotchas.

## SQLite-specific behaviors to fix

**Case sensitivity:** SQLite string comparisons are case-insensitive by default for ASCII characters. PostgreSQL is case-sensitive. Queries like `WHERE status = 'Active'` that worked in SQLite will miss rows with 'active' in PostgreSQL. Audit all string comparisons.

**Boolean type:** SQLite stores booleans as integers (0/1). PostgreSQL has a real boolean type. ORM-generated queries usually handle this but raw SQL won't.

**Integer primary keys:** SQLite's `INTEGER PRIMARY KEY` is auto-increment. PostgreSQL uses `SERIAL` or `GENERATED ALWAYS AS IDENTITY`. SQLAlchemy handles this transparently but raw migrations need updating.

**Date/time handling:** SQLite stores datetimes as strings. PostgreSQL has actual datetime types. The timezone behavior differs significantly — PostgreSQL's `TIMESTAMP WITH TIME ZONE` stores in UTC and converts on display.

**LIMIT without ORDER BY:** Technically undefined behavior in both, but SQLite tends to return consistent results anyway. PostgreSQL may return rows in any order when there's no ORDER BY. This exposed several bugs in code that assumed ordering.

## Migration process that worked

1. Dump SQLite schema, translate to PostgreSQL syntax manually
2. Use `sqlite3 db.db .dump` to export data as SQL inserts
3. Transform the INSERT statements (mostly type coercions) with a script
4. Load into a fresh PostgreSQL database
5. Run the test suite against the new database
6. Fix failures one by one

The test suite caught about 15 issues; manual review caught 3 more.

## What I wish I'd done earlier

Add `NOT NULL` constraints explicitly in SQLite from the start — SQLite allows NULLs unless you specify. PostgreSQL migrations surfaced several columns that had unexpected NULLs.

Reference: https://www.postgresql.org/docs/current/migration.html
