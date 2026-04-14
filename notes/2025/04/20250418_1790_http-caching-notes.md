# HTTP caching — reference notes

Notes on HTTP caching semantics, written after spending an afternoon debugging stale cache behavior in the v2 API.

## Core headers

**Cache-Control** is the primary directive header. Values compose:

```
Cache-Control: public, max-age=3600
Cache-Control: private, no-store
Cache-Control: no-cache          # revalidate before use
Cache-Control: no-store          # don't cache at all
Cache-Control: must-revalidate   # respect max-age strictly
Cache-Control: s-maxage=86400    # shared cache max age (overrides max-age for CDNs)
```

- `public`: can be cached by any cache (including CDNs)
- `private`: only in browser, not shared caches

**ETag:** Server provides an opaque identifier for the resource version. On subsequent requests, client sends `If-None-Match: "etag-value"`. Server returns `304 Not Modified` if unchanged.

**Last-Modified / If-Modified-Since:** Date-based equivalent. Less precise than ETags.

## Validation responses

```
HTTP/1.1 304 Not Modified
ETag: "abc123"
Cache-Control: max-age=3600
```

304 should not include a body. Client uses its cached version.

## Vary header

```
Vary: Accept-Encoding
Vary: Authorization
```

Tells caches that responses vary by the specified request headers. A response with `Vary: Authorization` should not be cached by shared caches (CDNs), since different users get different content. A common mistake is caching authenticated API responses at the CDN layer.

## What I fixed

The v2 API was returning `Cache-Control: max-age=300` on some authenticated endpoints. Changed to `Cache-Control: private, no-store` for all auth-gated resources. Also added `Vary: Authorization` as defense-in-depth.

RFC 9111 (HTTP caching): [https://datatracker.ietf.org/doc/html/rfc9111](https://datatracker.ietf.org/doc/html/rfc9111)
