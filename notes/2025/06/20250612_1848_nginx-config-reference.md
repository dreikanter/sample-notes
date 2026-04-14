---
title: Nginx configuration snippets I keep looking up
slug: nginx-config-reference
tags: [nginx, devops, reference]
description: Common nginx config patterns for quick reference
public: true
---

# Nginx configuration snippets I keep looking up

**Reverse proxy with upstream**

```nginx
upstream app {
    server 127.0.0.1:3000;
    keepalive 16;
}

server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://app;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

**Rate limiting**

```nginx
# In http block
limit_req_zone $binary_remote_addr zone=api:10m rate=30r/m;

# In location block
limit_req zone=api burst=10 nodelay;
limit_req_status 429;
```

**Gzip compression**

```nginx
gzip on;
gzip_vary on;
gzip_types text/plain text/css application/json application/javascript
           text/xml application/xml application/xml+rss text/javascript;
gzip_min_length 1000;
```

**Security headers**

```nginx
add_header X-Frame-Options "SAMEORIGIN" always;
add_header X-Content-Type-Options "nosniff" always;
add_header Referrer-Policy "strict-origin-when-cross-origin" always;
add_header Permissions-Policy "camera=(), microphone=(), geolocation=()" always;
```

**Redirect www to non-www**

```nginx
server {
    listen 80;
    server_name www.example.com;
    return 301 $scheme://example.com$request_uri;
}
```

Full documentation: https://nginx.org/en/docs/
