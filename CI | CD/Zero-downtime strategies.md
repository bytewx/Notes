# Zero-downtime strategies

1) Nginx stays, app rolls
- Keep nginx container persistent.
- Update app (web, php) with Compose rolling replace.
- Reload Nginx (HUP) only if config changed: nginx -s reload is hot, no dropped conns.

2) Blue/Green with upstream switch
- nginx/conf.d/upstream.conf:
```
map $cookie_release $active_pool {
  default blue;
  green green;
}
upstream web_blue  { server web_blue:8080; }
upstream web_green { server web_green:8080; }

server {
  listen 80;
  # ...
  location / {
    proxy_pass http://web_$active_pool;
  }
}
```

## Deploy
- Bring up web_green with new tag.
- Verify health.
- Switch cookie or a temporary map default to green.
- Decommission blue.

## Healthchecks
- Expose /healthz in app; in Compose:
```
healthcheck:
  test: ["CMD", "curl", "-sf", "http://localhost:8080/healthz"]
  interval: 10s
  timeout: 3s
  retries: 6
```
