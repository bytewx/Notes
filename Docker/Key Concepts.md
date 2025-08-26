# Key Concepts

- Image: read-only filesystem + metadata (build once, run many).
- Container: runnable instance of an image (ephemeral).
- Registry: remote storage for images (Docker Hub, GHCR).
- Bind mount: -v $(pwd):/app – live edit code from host inside container (dev).
- Named volume: -v db-data:/var/lib/mysql – Docker-managed persistent data.
- ARG vs ENV:
- ARG: build-time only.
- ENV: baked into image; available at runtime.
- EXPOSE: documentation only; use -p or ports: to publish.
