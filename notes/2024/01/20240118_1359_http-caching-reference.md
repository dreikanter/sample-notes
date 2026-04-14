# HTTP caching reference

Revisiting this for the CDN configuration work this week. Always more subtle than I remember.

## Key headers

**Cache-Control** — the main directive:
```
Cache-Control: max-age=3600                  # cache for 1 hour
Cache-Control: no-cache                      # revalidate before using cached copy
Cache-Control: no-store                      # don't cache at all
Cache-Control: private                       # only browser can cache, not CDN
Cache-Control: public, max-age=86400        # CDN can cache for 1 day
Cache-Control: stale-while-revalidate=60    # serve stale while fetching fresh
```

`no-cache` does not mean "don't cache." It means "revalidate with the server before using the cached copy." I have explained this four times this year.

**ETag:** Server assigns a version identifier to a resource. Browser sends it back as `If-None-Match: "etag-value"`. If unchanged, server responds 304 Not Modified with no body. Saves bandwidth.

**Last-Modified / If-Modified-Since:** Same pattern but using timestamps rather than opaque identifiers. ETags are more reliable when the content might regenerate identically with a different timestamp.

## Cache hierarchy

Browser cache → CDN → origin server. Requests travel down the chain until they get a hit. The CDN cache reduces origin load. The browser cache eliminates network round trips entirely.

## Common mistakes

- Setting `Cache-Control: public` on authenticated responses (data leak via shared CDN cache)
- Not setting `Vary: Accept-Encoding` when serving both gzipped and non-gzipped content (caches serve the wrong version)
- Setting `max-age=0` when you mean `no-store` (still allows conditional revalidation)

Reference: [MDN HTTP caching guide](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching) is comprehensive and correct.
