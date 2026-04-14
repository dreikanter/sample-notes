# HTTP caching headers — reference

Always mess these up. Writing them down properly.

**Cache-Control directives**

| Directive | Meaning |
|-----------|---------|
| `max-age=N` | Cache for N seconds |
| `no-cache` | Revalidate with server before using cached copy |
| `no-store` | Do not cache at all |
| `private` | Only cache in browser, not CDN/proxy |
| `public` | Can be cached by any cache |
| `immutable` | Resource will never change; skip revalidation even on reload |
| `stale-while-revalidate=N` | Serve stale while revalidating for up to N seconds |
| `must-revalidate` | Never serve stale; block until revalidation complete |

**Common patterns**

Versioned static assets (JS, CSS with content hash in filename):
```
Cache-Control: public, max-age=31536000, immutable
```

HTML documents (frequently changing):
```
Cache-Control: no-cache
```

API responses (dynamic, private):
```
Cache-Control: private, no-store
```

API responses (cacheable with stale tolerance):
```
Cache-Control: public, max-age=60, stale-while-revalidate=300
```

**ETag and Last-Modified**

For conditional requests — the server sends `ETag: "abc123"` and the browser sends `If-None-Match: "abc123"` on subsequent requests. Server returns 304 Not Modified if unchanged.

`Vary` header matters: `Vary: Accept-Encoding` means CDNs must cache separate copies per encoding.

Full reference: [MDN HTTP caching guide](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)
