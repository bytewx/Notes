# PHP-FPM

```
server {
  listen 80;
  server_name php.example.local;
  root /var/www/phpapp/public;
  index index.php index.html;

  location / {
    try_files $uri $uri/ /index.php?$query_string;
  }

  location ~ \.php$ {
    include snippets/fastcgi-php.conf; # sets SCRIPT_FILENAME properly
    fastcgi_pass unix:/run/php/php8.2-fpm.sock; # adjust version/socket
    fastcgi_read_timeout 60s;
  }

  location ~* \.(jpg|jpeg|png|gif|ico|css|js|svg|woff2?)$ {
    expires 7d;
    access_log off;
  }
}
```

- PHP-FPM on Ubuntu uses sockets like /run/php/phpX.Y-fpm.sock. On some distros use 127.0.0.1:9000.
