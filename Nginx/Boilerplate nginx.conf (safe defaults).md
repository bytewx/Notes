# Boilerplate nginx.conf (safe defaults)

```
user www-data;
worker_processes auto;
pid /run/nginx.pid;

events { worker_connections 2048; }

http {
  include       /etc/nginx/mime.types;
  default_type  application/octet-stream;

  sendfile on;
  tcp_nopush on;
  tcp_nodelay on;

  keepalive_timeout 65;
  server_tokens off;

  # Logs
  access_log /var/log/nginx/access.log;
  error_log  /var/log/nginx/error.log warn;

  # Gzip/Brotli and map for WebSockets
  gzip on;
  map $http_upgrade $connection_upgrade { default upgrade; '' close; }

  include /etc/nginx/conf.d/*.conf;
  include /etc/nginx/sites-enabled/*;
}
```
