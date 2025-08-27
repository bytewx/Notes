# Docker Compose (Dev & Prod)

```
services:
  api:
    build: .
    image: yourname/your-app:dev
    ports: ["3000:3000"]
    environment:
      NODE_ENV: development
    volumes:
      - ./:/app
    command: sh -lc "npm ci && npm run dev"

  db:
    image: mysql:8.4
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: appdb
    volumes:
      - db-data:/var/lib/mysql

volumes:
  db-data:
```

Commands:

```
docker compose up -d --build
docker compose ps
docker compose logs -f api
docker compose exec api sh
docker compose down         # stop / remove
docker compose down -v      # also remove volumes (data loss!)
```
