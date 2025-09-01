# Minimal Static Site

```
server {
  listen 80;
  server_name example.com;
  root /var/www/example.com/public;
  index index.html;

  location / {
    try_files $uri $uri/ =404;
  }

  access_log /var/log/nginx/example_access.log;
  error_log  /var/log/nginx/example_error.log warn;
}
```

- Enable & test:
```
sudo ln -s /etc/nginx/sites-available/example.conf /etc/nginx/sites-enabled/
nginx -t && sudo systemctl reload nginx
```
