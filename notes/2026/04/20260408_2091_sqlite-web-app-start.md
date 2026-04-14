---
title: SQLite web app – starting notes
slug: sqlite-web-app-start
tags: [projects, go, sqlite, web]
description: Notes from starting the SQLite-backed personal web application
---

# SQLite web app – starting notes

From the Q2 priority list (see [[20260330_2082]]): start the SQLite personal web app. This is a companion to the static site generator — a small CMS/admin interface for managing notes and pages.

## Stack

- Go (net/http, no framework — I want to understand the routing)
- SQLite via `modernc.org/sqlite` (pure Go driver, no CGo)
- HTMX for interactivity (avoid JavaScript complexity)
- HTML templates with Go's `html/template`

## Schema (first pass)

```sql
CREATE TABLE pages (
    id          INTEGER PRIMARY KEY,
    slug        TEXT UNIQUE NOT NULL,
    title       TEXT NOT NULL,
    content     TEXT,
    tags        TEXT,  -- JSON array
    published   INTEGER DEFAULT 0,
    created_at  TEXT DEFAULT (datetime('now')),
    updated_at  TEXT DEFAULT (datetime('now'))
);

CREATE INDEX idx_pages_slug ON pages(slug);
CREATE INDEX idx_pages_published ON pages(published);
```

Keeping it simple. Tags as a JSON array in a text column — no separate tag table for this scale.

## WAL mode on startup

```go
db.Exec("PRAGMA journal_mode=WAL")
db.Exec("PRAGMA synchronous=NORMAL")
db.Exec("PRAGMA foreign_keys=ON")
```

Per the research notes in [[20260323_2078]].

## HTMX pattern for inline editing

The idea: each page row in the admin table has an edit button. Clicking it swaps the row in-place with an edit form (HTMX `hx-get`), submits via form, swaps back with the updated row. No page reload.

Starting implementation today. Goal: working CRUD by end of the week.

HTMX docs: https://htmx.org/docs/
modernc sqlite: https://pkg.go.dev/modernc.org/sqlite
