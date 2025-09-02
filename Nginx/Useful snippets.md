# Useful snippets

- SPA fallback (React/Vue/etc.):
```
location / {
  try_files $uri /index.html;
}
```
- CORS (simple public API):
```
add_header Access-Control-Allow-Origin "*";
add_header Access-Control-Allow-Methods "GET, POST, OPTIONS";
add_header Access-Control-Allow-Headers "Content-Type, Authorization";
if ($request_method = OPTIONS) { return 204; }
```
- Large uploads (tune buffers/timeouts):
```
client_max_body_size 50m;
proxy_connect_timeout 5s;
proxy_send_timeout 60s;
proxy_read_timeout 60s;
send_timeout 60s;
```
- Logging & formats:
```
log_format main '$remote_addr - $remote_user [$time_local] '
                '"$request" $status $body_bytes_sent '
                '"$http_referer" "$http_user_agent" '
                '$request_time $upstream_response_time';

access_log /var/log/nginx/access.log main;
error_log  /var/log/nginx/error.log warn;
```
