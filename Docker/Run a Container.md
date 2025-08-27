# Run a Container

```
# Map host port 3000 -> container 3000
docker run --rm -it -p 3000:3000 yourname/your-app:dev
```

- --rm: auto-remove when it exits
- -it: interactive TTY (useful for shells)
