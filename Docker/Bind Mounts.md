# Bind Mounts

Edit code on host, see changes in the container immediately.

```
docker run --rm -it \
  -p 3000:3000 \
  -v "$PWD":/app \
  -w /app \
  node:20-alpine sh -lc "npm ci && npm run dev"
```
