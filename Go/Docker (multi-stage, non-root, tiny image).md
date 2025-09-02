# Docker (multi-stage, non-root, tiny image)

```
# Dockerfile
FROM golang:1.22 AS build
WORKDIR /src
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 \
go build -trimpath -ldflags "-s -w -X main.version=$(git rev-parse --short HEAD)" \
-o /out/app ./cmd/app


# Distroless or scratch runtime
FROM gcr.io/distroless/static:nonroot
WORKDIR /
COPY --from=build /out/app /app
USER nonroot:nonroot
EXPOSE 8080
ENTRYPOINT ["/app"]
HEALTHCHECK --interval=30s --timeout=3s CMD ["/app", "-healthcheck"]
```
