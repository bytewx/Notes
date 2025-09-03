# Best-practice checklist

- Right layer for the job (L4 vs L7).
- Choose algorithm for workload shape (least-conn/EWMA for variable duration).
- Active + passive health, outlier ejection, slow-start.
- Timeouts everywhere; retries only when safe.
- Prefer stateless; if sticky, cookie/hash + external session store.
- Connection pooling/keep-alive tuned (clients and LB).
- Observability: logs, metrics, traces tied to request IDs.
- Release safety: blue/green or canary + fast rollback + draining.
- Capacity & weights reflect real instance sizes; avoid skew.
- Document SLOs and on-call runbooks (what to flip first).
