# Main commands

- Test & reload:
```
nginx -t
sudo nginx -s reload # or: sudo systemctl reload nginx
```

- Quick start (Ubuntu/Debian):
```
sudo apt update && sudo apt install -y nginx
sudo systemctl enable --now nginx
```

- Log tails:
```
sudo tail -f /var/log/nginx/access.log /var/log/nginx/error.log
```

- Where configs live:
- Main:     /etc/nginx/nginx.conf
- Sites:    /etc/nginx/sites-available/*.conf  -> symlink -> /etc/nginx/sites-enabled/
- Snippets: /etc/nginx/snippets/*.conf
