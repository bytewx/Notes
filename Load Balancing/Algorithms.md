# Algorithms

- Round-robin / Weighted RR — simple; weight by capacity (CPU/mem).
- Least-connections — favors less busy instances (great for long-lived requests).
- Least-response-time / EWMA — favors faster instances by latency (HAProxy/Envoy).
- Power of Two Choices (P2C) — sample 2 at random, pick the less loaded → near-optimal with low overhead.
- Consistent hashing (IP/Key) — same key → same backend; minimizes rehash churn on scale up/down (caches).
- Ring-hash/Maglev — production-grade consistent hashing variants (Envoy/GCLB style).

- Rule of thumb: start WRR; if requests vary in length, prefer least-conn; for tail latency, use EWMA/least-time; for sticky data/caches, consistent hashing.
