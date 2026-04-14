---
title: HTTP caching headers explained
slug: http-caching-headers-explained
tags: [http, caching, web, backend]
description: Clear explanation of Cache-Control and related headers
public: true
---

# HTTP caching headers explained

Getting confused by these again. Writing it down properly.

## Cache-Control directives

```
Cache-Control: max-age=3600
```
Response can be cached for 3600 seconds before revalidation.

```
Cache-Control: no-cache
```
Despite the name, this doesn't mean "don't cache." It means "revalidate with the server before using cached copy." The server can respond with 304 Not Modified and the browser uses its cached copy.

```
Cache-Control: no-store
```
This actually means don't cache: never store this response. Use for sensitive data.

```
Cache-Control: public
```
Can be cached by any cache (including CDNs, proxies). Safe for static assets.

```
Cache-Control: private
```
Only the browser should cache this. CDNs and shared caches should not store it.

```
Cache-Control: immutable
```
The response will never change. Browser won't revalidate even if the user force-refreshes. Use with versioned assets (e.g., `main.a3b4c5.js`).

## ETag and conditional requests

```
ETag: "abc123"
If-None-Match: "abc123"
```

Server generates an ETag (content hash or version). Browser sends it back on next request. If content hasn't changed, server responds 304 (no body, saves bandwidth).

## Practical pattern for static assets

```
Cache-Control: public, max-age=31536000, immutable
```

One year, immutable. Only works if filenames are content-hashed. Change the content, change the filename.

[MDN Cache-Control reference](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control)
