# Cheatsheet

```
# Build
docker build -t repo/app:dev .

# Run (publish port)
docker run --rm -it -p 3000:3000 repo/app:dev

# Compose up/down
docker compose up -d --build
docker compose ps
docker compose logs -f api
docker compose down            # stop/remove
docker compose down -v         # also remove volumes

# Inspect / debug
docker images
docker ps -a
docker exec -it <container> sh
docker inspect <container_or_image>
docker top <container>
docker stats

# Cleanup
docker image prune -f
docker builder prune -f
docker system df

# Registry
docker login
docker tag repo/app:dev repo/app:1.0.0
docker push repo/app:1.0.0
```
