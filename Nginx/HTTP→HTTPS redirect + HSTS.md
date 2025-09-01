# HTTP→HTTPS redirect + HSTS

```
server {
  listen 80;
  server_name example.com www.example.com;
  return 301 https://example.com$request_uri;
}

server {
  listen 443 ssl http2;
  server_name example.com;

  ssl_certificate     /etc/letsencrypt/live/example.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

  add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;

  root /var/www/example.com/public;
  index index.html;

  location / { try_files $uri $uri/ =404; }
}
```

- Let’s Encrypt (certbot) quickie:
```
sudo apt install -y certbot python3-certbot-nginx
sudo certbot --nginx -d example.com -d www.example.com
# auto-renews via systemd timer/cron

```
