# Main Points

- Why LB? Distribute traffic, hide failures, scale horizontally, terminate TLS, centralize policy/observability.
- Layers: L4 (TCP/UDP, fast, blind) vs L7 (HTTP/gRPC aware, smart routing).
- Algorithms: round-robin, weighted RR, least-connections, least-response-time/EWMA, consistent hashing, power-of-two-choices.
- Health: active (probes) + passive (errors/timeouts). Outlier detection. Slow-start new/back-from-failure nodes.
- State: prefer stateless; if sticky, use cookie/IP hash and external session stores (Redis).
- Deploys: blue/green, canary (weighted), connection draining.
- Gotchas: DNS TTL/caching, retry storms, uneven capacity, cold starts, 502/503/504/499 debugging.
