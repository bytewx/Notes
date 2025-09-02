# Rate limiting (basic)

```
# http { ... }
limit_req_zone $binary_remote_addr zone=one:10m rate=5r/s;

server {
  # ...
  location /api/ {
    limit_req zone=one burst=10 nodelay;
    proxy_pass http://127.0.0.1:8080;
  }
}
```
