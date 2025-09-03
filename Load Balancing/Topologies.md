# Topologies

- Edge anycast → regional L7 → services.
- Active-active multi-region with DNS or global LB; data layer must support it (multi-primary / read replicas).
- HA pair (two LBs) per zone and N backends; use health checks + quorum.
