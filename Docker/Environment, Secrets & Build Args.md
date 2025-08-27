# Environment, Secrets & Build Args

- Build-time: ARG APP_VERSION → docker build --build-arg APP_VERSION=1.2.3 .
- Runtime: ENV in Dockerfile or environment: / --env in run/compose.
- Secrets (avoid baking into images):
- With BuildKit:

```
docker build --secret id=npmrc,src=$HOME/.npmrc -t your/app .

# syntax=docker/dockerfile:1.6
RUN --mount=type=secret,id=npmrc,dst=/root/.npmrc npm ci
```

- docker-compose.yml (runtime):

```
secrets:
  db_password:
    file: ./secrets/db_password.txt
services:
  api:
    secrets: [db_password]
```
