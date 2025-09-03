# Health & Resilience

- Active health checks: periodic HTTP/TCP probe (/healthz) → mark unhealthy.
- Passive checks: observe failures/timeouts → temporarily eject.
- Outlier detection: automatically eject a host that deviates (5xx, high latency).
- Slow-start / warm-up: ramp weight over N seconds after (re)join to avoid cold-start thundering herds.
- Connection draining: stop new conns, finish in-flight before remove/roll.
