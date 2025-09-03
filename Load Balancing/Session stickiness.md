# Session stickiness

- HTTP cookie (L7): set/load balancer cookie → same backend.
- IP hash (L4/L7): coarse; breaks with NAT or mobile clients.
- Header/JWT key (L7): consistent hash on userID/tenant.
- If you scale, put sessions in Redis/Memcached and keep backends stateless; stickiness becomes an optimization, not a requirement.
