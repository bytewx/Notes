# Multi-Stage Builds

Example: Node build → distroless runtime.

```
# 1) Build stage
FROM node:20-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# 2) Runtime stage (no package manager, smaller surface)
FROM gcr.io/distroless/nodejs20
WORKDIR /app
COPY --from=build /app/dist ./dist
ENV NODE_ENV=production
EXPOSE 3000
CMD ["dist/server.js"]
```

Benefits: smaller image, fewer CVEs, faster pulls.
