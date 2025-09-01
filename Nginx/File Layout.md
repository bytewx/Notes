# File Layout

```
/etc/nginx/
  ├─ nginx.conf            # includes /etc/nginx/sites-enabled/*
  ├─ conf.d/*.conf         # auto-included (if used by distro)
  ├─ sites-available/*.conf
  ├─ sites-enabled/*.conf  # symlinks to sites-available
  ├─ snippets/*.conf
  └─ mime.types
/var/www/<site>/public     # your static files / app entry
```
