# Compression, caching, and static hardening

```
# http { ... }
gzip on;
gzip_comp_level 5;
gzip_min_length 1024;
gzip_types text/plain text/css application/javascript application/json image/svg+xml;

# If you have the brotli module:
# brotli on;
# brotli_comp_level 5;
# brotli_types text/plain text/css application/javascript application/json image/svg+xml;

# Common cache rules
location ~* \.(?:css|js)$    { expires 7d;  add_header Cache-Control "public"; }
location ~* \.(?:png|jpg|jpeg|gif|svg|ico|webp|avif|woff2?)$ { expires 30d; add_header Cache-Control "public, immutable"; access_log off; }
```
