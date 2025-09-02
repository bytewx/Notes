# Performance tips

- Prefer try_files over regex where possible.
- Avoid if inside location (use map).
- Use keepalive for upstreams; enable HTTP/2 on TLS.
- Serve static assets directly from Nginx with long cache.
- Turn off access_log for heavy static paths.
- Check worker_processes auto; worker_connections 1024; (or higher per load).
