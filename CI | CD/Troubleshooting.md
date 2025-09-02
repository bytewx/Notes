# Troubleshooting

```
# CI logs
docker logs <container> --since 30m
# Nginx sanity
docker exec nginx nginx -T | less
# Network from nginx to upstream
docker exec nginx curl -I http://web:8080/healthz -v
# Compose diagnostics
docker compose ps && docker compose events
```
