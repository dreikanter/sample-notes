# Nginx config patterns

Quick reference for configurations I use or keep rebuilding from scratch.

## Basic reverse proxy

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
    }
}
```

## SSL with Let's Encrypt (certbot adds this)

```nginx
listen 443 ssl;
ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
include /etc/letsencrypt/options-ssl-nginx.conf;
```

## Static files with caching

```nginx
location /static/ {
    root /var/www/myapp;
    expires 1y;
    add_header Cache-Control "public, immutable";
    access_log off;
}
```

## Rate limiting

```nginx
# In http block
limit_req_zone $binary_remote_addr zone=api:10m rate=10r/s;

# In location block
location /api/ {
    limit_req zone=api burst=20 nodelay;
    proxy_pass http://backend;
}
```

## Gzip

```nginx
gzip on;
gzip_vary on;
gzip_types text/plain text/css application/json application/javascript text/xml;
gzip_min_length 1024;
```

## Testing config

```bash
nginx -t          # test syntax
nginx -s reload   # reload without downtime
```

Docs: https://nginx.org/en/docs/
