# Docker + Nginx (reverse proxy for containerized app)

- docker-compose.yml:
```
services:
  app:
    image: ghcr.io/acme/app:latest
    expose:
      - "8080"
  nginx:
    image: nginx:stable
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/conf.d:/etc/nginx/conf.d:ro
      - ./nginx/snippets:/etc/nginx/snippets:ro
      - ./www:/var/www:ro
      - ./certs:/etc/letsencrypt:ro
    depends_on:
      - app
```
- nginx/conf.d/app.conf:
```
upstream app { server app:8080; }

server {
  listen 80;
  server_name _;
  location / { proxy_pass http://app; }
}
```
