# Configure Service

```
sudo nano /etc/nginx/sites-available/service.conf
```
- Nginx configuration:
```
server {
    listen 80;
    server_name service.local;

    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header Referrer-Policy "strict-origin-when-cross-origin" always;
    add_header Content-Security-Policy "default-src 'self';" always;

    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        client_max_body_size 10M;
    }
}
```
- Activate service:
```
sudo ln -s /etc/nginx/sites-available/news.conf /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```
- Disable nginx's version display:
```
server_tokens off;
```
