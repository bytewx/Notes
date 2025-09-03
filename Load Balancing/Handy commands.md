# Handy commands

```
# Nginx quick sanity
nginx -t && nginx -T | less
tail -f /var/log/nginx/{access,error}.log

# HAProxy stats
echo "show servers state" | socat stdio /var/run/haproxy/admin.sock

# Envoy clusters & stats
curl localhost:9901/clusters | head
curl localhost:9901/stats | grep upstream

# Smoke test latency
hey -z 30s -q 100 http://api.example.com/healthz
```
