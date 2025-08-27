# Security Hardening

- Run as non-root:

```
RUN addgroup -g 10001 app && adduser -D -G app -u 10001 app
USER app
```

- Prefer distroless / minimal base in production.
- Limit Linux capabilities and set read-only FS when possible:

```
services:
  api:
    read_only: true
    cap_drop: ["ALL"]
    security_opt:
      - no-new-privileges:true
    tmpfs: ["/tmp"]
```

- Keep images updated; scan with docker scan or CI scanners.
