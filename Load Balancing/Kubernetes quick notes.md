# Kubernetes quick notes

- Service (ClusterIP) balances at L4 across pods ready endpoints (iptables/IPVS/eBPF).
- Ingress + Ingress Controller (e.g., Nginx/HAProxy/Envoy) provides L7 routing.
- Service types: ClusterIP (internal), NodePort, LoadBalancer (cloud LB per service), Gateway API (modern).
- ReadinessProbes gate inclusion in LB; PodDisruptionBudget + maxUnavailable control rollout blast radius.
- Stickiness: sessionAffinity: ClientIP at Service; cookie-based via specific ingress annotations (controller-specific).
