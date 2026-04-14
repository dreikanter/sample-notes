# HTTP caching headers reference

Things I have to relearn every few months. [MDN HTTP caching guide](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching) is the definitive reference.

## The main headers

**Cache-Control** (response): controls caching behaviour
```
Cache-Control: max-age=3600          # Cache for 1 hour
Cache-Control: no-cache              # Must revalidate before each use
Cache-Control: no-store              # Never cache (sensitive data)
Cache-Control: private               # Browser only, not CDN/proxy
Cache-Control: public                # Cacheable by CDN/proxy
Cache-Control: s-maxage=86400        # CDN cache duration (overrides max-age for shared caches)
Cache-Control: stale-while-revalidate=60  # Serve stale while refreshing in background
Cache-Control: immutable             # Content won't change (for fingerprinted assets)
```

**ETag** (response): fingerprint of the content
```
ETag: "d41d8cd98f00b204e9800998ecf8427e"
```

**Last-Modified** (response): when the content last changed
```
Last-Modified: Tue, 22 Oct 2024 10:00:00 GMT
```

**If-None-Match** (request): conditional request using ETag
```
If-None-Match: "d41d8cd98f00b204e9800998ecf8427e"
```
If content unchanged → 304 Not Modified (empty body). If changed → 200 with new content and new ETag.

## Common patterns

**Static assets with content hash** (JS, CSS):
```
Cache-Control: public, max-age=31536000, immutable
```
The filename hash busts the cache when content changes.

**HTML pages**:
```
Cache-Control: no-cache
ETag: "..."
```
No-cache means "always revalidate"; ETag means revalidation can return 304 if unchanged.

**API responses**:
```
Cache-Control: private, max-age=60
```
Or no caching for real-time data.

## Key confusion point

`no-cache` does NOT mean "don't cache". It means "cache it, but always check with the server before using it". `no-store` means actually don't cache.
