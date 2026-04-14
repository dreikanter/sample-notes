# API versioning strategies

Notes from working on the compatibility matrix for the notification API (see [[20251013_1968]]).

## The three main approaches

### 1. URL path versioning

```
/api/v1/users
/api/v2/users
```

Most common. Explicit, cache-friendly, easy to route in infrastructure. Downside: clients must update URLs when migrating, and maintaining multiple major versions requires real engineering overhead.

### 2. Header versioning

```
Accept: application/vnd.myapp.v2+json
API-Version: 2025-10-15
```

Keeps URLs clean. The date-based approach (used by Stripe and others) is interesting: you pin your client to a specific API date, and the server translates to the current implementation. Breaking changes are never exposed to existing clients automatically.

### 3. Query parameter versioning

```
/api/users?version=2
```

The worst option. Pollutes URLs, often breaks caching, and makes routing complex.

## Additive vs. breaking changes

**Additive (safe to deploy without versioning):**
- New endpoints
- New optional fields in responses
- New optional query parameters

**Breaking (require versioning or long deprecation window):**
- Removing fields from responses
- Changing field types
- Changing error formats
- Making optional fields required
- Changing authentication behavior

## Practical advice

- Serialize with care: if you add a field, clients that ignore unknown fields will be fine. Build clients to ignore what they don't understand.
- Sunset headers: return `Sunset: Sat, 01 Mar 2026 00:00:00 GMT` when deprecating a version.
- Give at least 6 months notice for breaking changes.

Reference: [Stripe API versioning documentation](https://stripe.com/docs/api/versioning) is the gold standard in the field.
