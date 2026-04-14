---
title: HTTP security headers reference
slug: security-headers-ref
tags: [security, web, reference]
---

# HTTP security headers reference

Headers to set on web applications for baseline security. Quick reference for new projects.

**Content-Security-Policy (CSP)**

Controls which resources can be loaded. The most powerful but also the most complex to configure. Start with a report-only policy to understand what would break before enforcing.

```
Content-Security-Policy: default-src 'self'; script-src 'self' 'nonce-{RANDOM}'; style-src 'self' 'unsafe-inline'; img-src 'self' data: https:; report-uri /csp-report
```

Avoid `'unsafe-inline'` for scripts — it defeats most of CSP's value. Use nonces or hashes instead.

**Strict-Transport-Security (HSTS)**

Forces HTTPS for the specified duration. Include `includeSubDomains` if all subdomains support HTTPS. Submit to the HSTS preload list once you're confident.

```
Strict-Transport-Security: max-age=63072000; includeSubDomains; preload
```

**X-Frame-Options**

Prevents clickjacking by controlling whether the page can be embedded in a frame. Replaced by CSP `frame-ancestors` but still useful for older browsers.

```
X-Frame-Options: DENY
```

**Permissions-Policy**

Controls browser feature access. Especially important for camera, microphone, geolocation.

```
Permissions-Policy: camera=(), microphone=(), geolocation=(), payment=()
```

**X-Content-Type-Options**

Prevents MIME-type sniffing, which can turn uploaded files into executable scripts.

```
X-Content-Type-Options: nosniff
```

**Check your headers**

Use securityheaders.com to audit. Aim for A grade as baseline.

OWASP headers guidance: https://owasp.org/www-project-secure-headers/
