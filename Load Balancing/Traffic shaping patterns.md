# Traffic shaping patterns

- Blue/Green: two identical pools; flip route. Easy rollback.
- Canary: gradually shift 1%→5%→25%→100% by weight or header.
- A/B testing: route by cookie/header; collect metrics per cohort.
- Failover/priority: send to primary; overflow to secondary on saturation.
