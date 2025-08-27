# Entrypoint vs CMD & Signal Handling

- CMD: default arguments to execute.
- ENTRYPOINT: fixed executable + optional CMD args.
- Use an entrypoint script with exec so PID 1 receives signals (SIGTERM) properly:

```
COPY docker/entrypoint.sh /usr/local/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]
CMD ["node", "src/server.js"]
```

```
# docker/entrypoint.sh
#!/usr/bin/env sh
set -e
# Pre-start tasks here (migrations, etc.)
exec "$@"   # hand off to main process, preserving signals
```
