# Copy-paste configs

- Nginx (HTTP, least connections, passive health, slow-start):
```
upstream api {
  least_conn;
  server 10.0.0.11:8080 max_fails=3 fail_timeout=10s slow_start=30s;
  server 10.0.0.12:8080 max_fails=3 fail_timeout=10s slow_start=30s;
  # note: active HTTP checks require NGINX Plus (OSS has passive only)
}

map $http_upgrade $conn_upgrade { default upgrade; '' close; }

server {
  listen 80;
  server_name api.example.com;

  location / {
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection $conn_upgrade;

    proxy_connect_timeout 2s;
    proxy_read_timeout 15s;
    proxy_send_timeout 15s;

    proxy_next_upstream error timeout http_502 http_503 http_504;
    proxy_next_upstream_tries 2;

    proxy_pass http://api;
  }
}
```
- HAProxy (HTTP mode, active checks, leastconn, outlier ejection):
```
global
  maxconn 20000
defaults
  mode http
  option httplog
  timeout connect 2s
  timeout client  30s
  timeout server  30s
  default-server init-addr none inter 2s fastinter 500ms downinter 5s slowstart 30s

frontend fe_api
  bind :80
  default_backend be_api

backend be_api
  balance leastconn
  option httpchk GET /healthz
  http-check expect status 200
  server s1 10.0.0.11:8080 check slowstart 30s
  server s2 10.0.0.12:8080 check slowstart 30s
  # Outlier detection (simple): mark down on consecutive failures via 'check', or use stick-table + lua for advanced ejection
```
- Envoy (ring hash for stickiness, outlier detection, retries):
```
static_resources:
  clusters:
  - name: api
    type: STRICT_DNS
    lb_policy: RING_HASH
    ring_hash_lb_config: { minimum_ring_size: 1024 }
    outlier_detection:
      consecutive_5xx: 3
      interval: 2s
      base_ejection_time: 30s
      max_ejection_percent: 50
    load_assignment:
      cluster_name: api
      endpoints:
      - lb_endpoints:
        - endpoint: { address: { socket_address: { address: 10.0.0.11, port_value: 8080 } } }
        - endpoint: { address: { socket_address: { address: 10.0.0.12, port_value: 8080 } } }
  listeners:
  - name: http
    address: { socket_address: { address: 0.0.0.0, port_value: 80 } }
    filter_chains:
    - filters:
      - name: envoy.filters.network.http_connection_manager
        typed_config:
          "@type": type.googleapis.com/envoy.extensions.filters.network.http_connection_manager.v3.HttpConnectionManager
          stat_prefix: ingress_http
          route_config:
            name: local_route
            virtual_hosts:
            - name: api
              domains: ["*"]
              routes:
              - match: { prefix: "/" }
                route:
                  cluster: api
                  retry_policy:
                    retry_on: 5xx,reset,connect-failure
                    num_retries: 2
                    per_try_timeout: 2s
          http_filters:
          - name: envoy.filters.http.router
```
