---
title: API design principles I've come to hold
slug: api-design-principles
tags: [api, engineering, design, reference]
description: Design principles for HTTP APIs based on experience
---

# API design principles I've come to hold

These are not universal laws. They're positions I've arrived at through building APIs and being frustrated by the ones built by others.

## Use nouns in URLs, not verbs

```
GET  /invoices         ✓
GET  /getInvoices      ✗
POST /invoices         ✓ (create)
POST /createInvoice    ✗
```

The HTTP method is the verb. The resource is the noun.

## Consistent status codes

- 200: success, body contains resource
- 201: created, Location header points to new resource
- 204: success, no body (DELETE, some PUTs)
- 400: client error (invalid request)
- 401: unauthenticated (you need to log in)
- 403: unauthorized (you're logged in but can't do this)
- 404: resource not found
- 422: validation failed (use this over 400 for validation specifically)
- 429: rate limited
- 500: server error (our problem, not yours)

## Error bodies should be consistent

Pick a format and use it everywhere. RFC 7807 (Problem Details) is a good starting point:

```json
{
  "type": "/errors/validation-failed",
  "title": "Validation failed",
  "status": 422,
  "errors": [
    { "field": "email", "message": "Must be a valid email address" }
  ]
}
```

## Versioning

URL versioning (`/v1/invoices`) is explicit and cache-friendly. Header versioning (`Accept: application/vnd.api+json;version=2`) is cleaner in theory but harder to test and discover.

I prefer URL versioning despite its inelegance.

## Pagination

Cursor-based is usually better than offset-based for real-time data. Offset pagination is inconsistent when records are added or deleted between pages.

[Stripe's API design](https://stripe.com/docs/api) is the practical gold standard — worth reading their documentation as a design exercise.
