# Load balancing (round-robin)

```
upstream web_pool {
  server 10.0.0.11:8080;
  server 10.0.0.12:8080;
  # server ... weight=2 max_fails=3 fail_timeout=30s;
}
server {
  listen 80;
  server_name pool.example.com;
  location / { proxy_pass http://web_pool; }
}
```
