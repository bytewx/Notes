# Reverse proxy (Node, Go, etc.) with WebSockets

```
upstream app_upstream {
  server 127.0.0.1:3000;
  keepalive 32;
}

server {
  listen 80;
  server_name app.example.com;

  location / {
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;

    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection $connection_upgrade;

    proxy_read_timeout 60s;
    proxy_pass http://app_upstream;
  }
}

# Enable $connection_upgrade var (put in http{} or a snippet)
map $http_upgrade $connection_upgrade {
  default upgrade;
  ''      close;
}
```
