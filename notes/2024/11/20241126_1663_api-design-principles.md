---
title: API design principles — personal reference
slug: api-design-principles
tags: [api, design, backend, http]
public: true
---

# API design principles — personal reference

What I've settled on after building and consuming a lot of HTTP APIs. See also [Stripe's API design guide](https://stripe.com/blog/api-versioning) and the [Microsoft REST API guidelines](https://github.com/microsoft/api-guidelines).

## Resource naming

- Plural nouns for collections: `/users`, `/orders`, not `/user`, `/order`
- Nested resources for relationships: `/users/123/orders`
- Avoid verbs in URLs: prefer `POST /users` over `POST /createUser`
- Actions that don't fit CRUD: use a sub-resource or action noun: `POST /orders/456/cancel`

## HTTP methods

- `GET`: safe and idempotent, never changes state
- `POST`: creates a resource or triggers an action, not idempotent
- `PUT`: replaces a resource entirely, idempotent
- `PATCH`: partial update, idempotent (should be)
- `DELETE`: removes a resource, idempotent

## Response structure

Be consistent. Either always use an envelope (`{ "data": {...}, "meta": {...} }`) or never do. Mixing is worse than either choice.

For errors, always include: a machine-readable error code, a human-readable message, optionally a pointer to which field caused the problem.

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Email address is invalid",
    "field": "email"
  }
}
```

## Versioning

Version in the URL (`/v1/`) rather than headers — it's more visible, easier to test, simpler to route. Never break an existing version; deprecate and add a new one.

## Pagination

Cursor-based pagination scales better than offset-based for large datasets. Offset pagination breaks when rows are inserted during traversal.

## Documentation

An undocumented API is a broken API. OpenAPI/Swagger is the de facto standard. Generate docs from the spec, not the code.
