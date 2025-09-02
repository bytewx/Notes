# Pitfalls

- proxy_pass trailing slash matters:
- location /api/ { proxy_pass http://up/; } → keeps /api/ suffix trimmed.
- location /api/ { proxy_pass http://up; } → passes full URI.
- root vs alias inside location: with alias, the location path is replaced.
- Ensure correct SCRIPT_FILENAME when using PHP-FPM (use distro snippets).
