# Cloud vs OSS

- Cloud: AWS (ALB L7, NLB L4), GCP (Global HTTP(S) L7, TCP/UDP L4), Azure equivalents. Pros: managed, global; Cons: fewer tweak knobs, cost.
- OSS: Nginx, HAProxy, Envoy. Pros: control, features; Cons: you manage HA, upgrades, observability.
