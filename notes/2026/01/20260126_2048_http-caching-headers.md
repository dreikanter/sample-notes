# HTTP caching headers reference

This trips me up every time. Keeping a reference.

## Cache-Control

The main directive. Can appear in both request and response headers.

```
Cache-Control: max-age=3600          # cache for 1 hour
Cache-Control: no-cache              # must revalidate before using cached copy
Cache-Control: no-store              # don't cache at all
Cache-Control: public                # shared caches (CDNs) may cache
Cache-Control: private               # only browser cache, not CDNs
Cache-Control: must-revalidate       # after max-age, must revalidate (don't serve stale)
Cache-Control: stale-while-revalidate=60  # serve stale for up to 60s while fetching fresh
```

`no-cache` is confusing: it doesn't mean "don't cache." It means "cache it, but always check with the server before serving it." `no-store` is the "don't cache" directive.

## ETag and conditional requests

Server sends `ETag: "abc123"` with response. Client sends `If-None-Match: "abc123"` on subsequent requests. If content unchanged, server responds 304 Not Modified with no body. This saves bandwidth while ensuring freshness.

`Last-Modified` / `If-Modified-Since` works similarly but is based on timestamps. ETags are preferred — timestamps can have resolution issues.

## Vary

```
Vary: Accept-Encoding
Vary: Accept, Accept-Language
```

Tells caches that the response varies by these request headers. Important for content negotiation.

## Typical patterns

Static assets (hash in filename):
```
Cache-Control: public, max-age=31536000, immutable
```

HTML:
```
Cache-Control: no-cache
```

API responses:
```
Cache-Control: private, max-age=300
```

Full spec: https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching
