# Writing Dockerfile

Example: minimal web app (Node).

```
# Dockerfile
FROM node:20-alpine

# 1) Create app directory
WORKDIR /app

# 2) Install deps (leverage cache)
COPY package*.json ./
RUN npm ci

# 3) Copy source
COPY . .

# 4) Build (optional, if you have a build step)
# RUN npm run build

# 5) Document port and start
EXPOSE 3000
ENV NODE_ENV=production
CMD ["node", "src/server.js"]

```

Layering rule: put slow-changing files first (package.json) to maximize cache hits.
