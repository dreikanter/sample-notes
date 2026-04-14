---
title: API design — notes on consistency and versioning
slug: api-design-notes
tags: [api, programming, architecture]
description: Collected notes on REST API design decisions
---

# API design — notes on consistency and versioning

Notes from a few weeks of thinking about API design while working on a client integration project. Mostly REST-focused.

**Resource naming**

Use nouns, not verbs. `/users/123/orders`, not `/getOrdersForUser/123`. The HTTP method is already a verb. Mixing verbs into paths creates redundancy and inconsistency.

Plural resource names consistently: `/users`, not `/user`. The resource collection is plural even when fetching a single item (`/users/123`).

Avoid deep nesting beyond two levels. `/users/123/orders/456` is fine; `/users/123/orders/456/items/789/reviews` is a sign you need to rethink the hierarchy.

**Status codes — the ones people get wrong**

- `200` for GETs that return data, `201` for POST that creates a resource, `204` for successful DELETE with no body
- `400` for client validation errors with a body explaining what's wrong
- `401` for unauthenticated, `403` for authenticated-but-unauthorized — these are different things
- `409` for conflicts (trying to create something that already exists)
- `422` for semantically invalid requests (well-formed JSON but fails business rules)

**Versioning**

URL versioning (`/v1/users`) is the most visible and easiest for clients to handle. Header versioning (`Accept: application/vnd.myapi.v1+json`) is more RESTfully correct but harder to debug and test.

Pick a strategy and stick to it from the start. Changing versioning approaches mid-lifecycle is painful.

**Pagination**

Cursor-based pagination scales better than offset-based. For large tables, `LIMIT 20 OFFSET 10000` means scanning 10,020 rows. A cursor (timestamp or ID-based) is a WHERE clause, not a skip.

REST API design guide: https://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api
