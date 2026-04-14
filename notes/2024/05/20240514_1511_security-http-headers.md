# Security HTTP headers reference

## Content-Security-Policy

The most important and complex one. Controls what resources can load.

```
Content-Security-Policy: default-src 'self'; 
  script-src 'self' https://cdn.example.com;
  style-src 'self' 'unsafe-inline';
  img-src 'self' data: https:;
  connect-src 'self' https://api.example.com;
  font-src 'self' https://fonts.gstatic.com;
  frame-ancestors 'none';
  base-uri 'self';
  form-action 'self';
```

Start with `Content-Security-Policy-Report-Only` to test without enforcement. Violations sent to `report-uri`.

## Strict-Transport-Security (HSTS)

```
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
```

Tells browsers to only use HTTPS for the domain for max-age seconds. `preload` is for the HSTS preload list — permanent, can't easily undo.

## X-Frame-Options

```
X-Frame-Options: DENY
X-Frame-Options: SAMEORIGIN
```

Deprecated in favor of CSP `frame-ancestors`, but still worth setting for older browsers.

## X-Content-Type-Options

```
X-Content-Type-Options: nosniff
```

Prevents MIME type sniffing. Browser uses declared Content-Type, not guesses.

## Referrer-Policy

```
Referrer-Policy: strict-origin-when-cross-origin
```

Controls how much referrer info is sent. `strict-origin-when-cross-origin` sends origin only for cross-origin requests, full URL for same-origin.

## Permissions-Policy

```
Permissions-Policy: camera=(), microphone=(), geolocation=()
```

Replaces Feature-Policy. Restricts access to browser features.

## CORS headers (for APIs)

```
Access-Control-Allow-Origin: https://app.example.com
Access-Control-Allow-Methods: GET, POST, OPTIONS
Access-Control-Allow-Headers: Content-Type, Authorization
Access-Control-Max-Age: 86400
```

Never use `Access-Control-Allow-Origin: *` with `Access-Control-Allow-Credentials: true`.

Testing tool: https://securityheaders.com/
