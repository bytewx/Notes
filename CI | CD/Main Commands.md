# Main Commands

```
# Build & tag image
docker build -t registry.example.com/app/web:$(git rev-parse --short HEAD) .

# Push
docker push registry.example.com/app/web:$(git rev-parse --short HEAD)

# Deploy (Compose on server)
ssh vps 'cd /srv/app && docker compose pull && docker compose up -d && docker exec nginx nginx -t && docker exec nginx nginx -s reload'

# Rollback (pick previous tag)
ssh vps 'cd /srv/app && APP_TAG=<prev> docker compose up -d && docker exec nginx nginx -s reload'
```
