# Debugging

```
nginx -T               # dump full config with includes
nginx -t               # syntax check
sudo tail -f /var/log/nginx/error.log
curl -I http://host/   # quick headers
curl -v http://host/   # verbose
```
