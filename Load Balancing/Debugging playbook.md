# Debugging playbook

1) Repro quickly: curl -vI http://host/endpoint (add --connect-timeout 2 --max-time 5).
2) Differentiate 5xx:
- 502: upstream connection failed/closed — check app, keep-alive, TLS mismatch.
- 503: no healthy upstreams / maxconn — scaling or health checks failing.
- 504: upstream timeout — app slow, timeout too small, DB contention.
- 499 (Nginx log): client aborted — usually latency too high; check p95.
3) Check health: LB health page, nginx -T, HAProxy stats, Envoy /clusters.
4) Capacity: per-instance load skew? Apply least-conn/EWMA or fix weights.
5) Warmups: enable slow-start; verify new pods not hammered.
6) Retries: ensure capped retries + backoff + idempotency only.
