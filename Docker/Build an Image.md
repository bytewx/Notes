# Build an Image

## Build with a tag (repository:tag)

```
docker build -t yourname/your-app:dev .
```

## With BuildKit progress:

```
DOCKER_BUILDKIT=1 docker build -t yourname/your-app:dev .
```
