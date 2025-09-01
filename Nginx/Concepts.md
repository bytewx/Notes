# Concepts

- http → server → location blocks process requests in layers.
- server_name chooses a vhost; listen 80; / listen 443 ssl; choose ports.
- location matches a URI; prefer location / + try_files over if.
- Reverse proxy with proxy_pass to upstream services.
- For PHP, use php-fpm via fastcgi_pass.
- Keep TLS, Gzip/Brotli, security headers enabled.
