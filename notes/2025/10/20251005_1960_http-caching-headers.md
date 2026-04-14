# HTTP caching headers — a working reference

Always confuse myself on this. Writing it down properly.

## Cache-Control

The primary header. Directives compose:

```
Cache-Control: public, max-age=86400
Cache-Control: private, no-cache
Cache-Control: no-store
Cache-Control: max-age=0, must-revalidate
```

- `public` — may be cached by shared caches (CDNs, proxies)
- `private` — only cached by browser, not shared caches
- `no-cache` — cached but must revalidate with server before each use
- `no-store` — don't cache at all (use for sensitive data)
- `max-age=N` — cache for N seconds
- `s-maxage=N` — like max-age but for shared caches only
- `must-revalidate` — don't serve stale even if server is down
- `immutable` — hint that content won't change during max-age period

## ETag and conditional requests

Server returns `ETag: "abc123"`. Client stores it. On next request:

```
If-None-Match: "abc123"
```

Server returns `304 Not Modified` (no body) if unchanged. Efficient.

## Last-Modified

Older mechanism:

```
Last-Modified: Thu, 01 Oct 2025 12:00:00 GMT
If-Modified-Since: Thu, 01 Oct 2025 12:00:00 GMT
```

ETags are preferred for precision; Last-Modified for systems without easy hash generation.

## Common patterns

**Static assets with content hash in filename:**
```
Cache-Control: public, max-age=31536000, immutable
```

**API responses (authenticated):**
```
Cache-Control: private, no-cache
```

**HTML pages:**
```
Cache-Control: public, max-age=0, must-revalidate
ETag: "page-version-hash"
```

Reference: [MDN HTTP caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)

