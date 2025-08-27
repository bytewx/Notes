# Common Pitfalls & Anti-patterns

- Using latest everywhere (pin base images).
- Copying the whole repo before installing deps (breaks cache).
- Running as root in production.
- Putting secrets in the image or .env committed to Git.
- Using localhost to reach other containers (use service names).
- Forgetting .dockerignore → huge build contexts, slow builds.
- Not handling signals (no exec in entrypoint).
- docker compose down -v accidentally nuking data.
