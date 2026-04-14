# HTTP caching headers reference

## Cache-Control directives

```
max-age=<seconds>       — cache for N seconds from request time
s-maxage=<seconds>      — same but for shared caches (CDNs), overrides max-age
no-cache                — revalidate with origin before each use (not "don't cache")
no-store                — don't cache at all (sensitive data)
private                 — browser can cache, CDN cannot
public                  — shared caches may cache
must-revalidate         — don't serve stale even if origin unreachable
immutable               — content will never change (use with content hashing)
stale-while-revalidate=N — serve stale up to N seconds while revalidating
stale-if-error=N        — serve stale up to N seconds if origin errors
```

## Common patterns

**Static assets with cache busting:**
```
Cache-Control: public, max-age=31536000, immutable
```
(Works because URL contains hash, so URL changes when content changes)

**HTML pages:**
```
Cache-Control: no-cache
```
(Forces revalidation, but allows 304 Not Modified to avoid re-transfer)

**Private API responses:**
```
Cache-Control: private, max-age=60
```

**Never cache:**
```
Cache-Control: no-store
```

## Validation headers

```
ETag: "abc123"          — opaque identifier for resource version
Last-Modified: <date>   — when resource was last modified

If-None-Match: "abc123"     — conditional GET with ETag
If-Modified-Since: <date>   — conditional GET with date
```

Server returns 304 Not Modified if unchanged — saves bandwidth, not latency.

## Vary header

```
Vary: Accept-Encoding
Vary: Accept, Accept-Language
```

Tells caches to store separate responses for different request header values. Often necessary for content negotiation.

Spec: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control
