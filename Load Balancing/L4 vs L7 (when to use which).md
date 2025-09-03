# L4 vs L7 (when to use which)

| Aspect                     | Layer 4 (Transport LB)                                  | Layer 7 (Application LB)                                             |
|----------------------------|----------------------------------------------------------|-----------------------------------------------------------------------|
| Protocol awareness         | TCP/UDP only (5-tuple)                                   | HTTP/1.1, HTTP/2, gRPC, WebSockets, headers, cookies, paths          |
| Performance/overhead       | Highest throughput, lowest CPU                           | High throughput, some CPU for parsing and policies                    |
| Routing features           | None beyond 5-tuple                                      | Host/path/header/cookie routing, canary, A/B, traffic shaping        |
| TLS handling               | Usually passthrough                                      | TLS termination, SNI, mTLS, re-encrypt to upstream                   |
| Stickiness                 | Client IP/port (coarse; NAT/mobile issues)               | Cookie/header/JWT based; consistent hashing by key                    |
| Health checks              | Basic TCP/port checks                                    | HTTP/gRPC active checks, outlier ejection, slow-start                 |
| Retries/timeouts           | Limited                                                  | Per-route retries, per-try timeouts, hedging                          |
| Observability              | Conn stats only                                          | Rich metrics (latency buckets), request logs, tracing                 |
| Cost/ops                   | Cheaper/simpler                                          | More features; more tuning/ops                                        |
| Typical tech               | AWS NLB, GCP TCP/UDP LB, iptables/eBPF                   | Nginx/HAProxy/Envoy, AWS ALB/GCLB HTTP(S)                             |
| Use it when                | Raw TCP/UDP, ultra-high throughput, minimal logic        | Smart HTTP routing, TLS offload, canary/A-B, multi-service gateways   |
| Examples                   | Databases, game servers, RTMP, MQTT                      | Web APIs, microservices, REST/gRPC, SPA backends                      |
