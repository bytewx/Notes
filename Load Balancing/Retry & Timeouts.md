# Retry & Timeouts (avoid retry storms)

- Set request and upstream timeouts (connect/read/write).
- Retry on: idempotent methods (GET/HEAD), safe gRPC codes; never blindly retry POST unless idempotency keys.
- Add jittered backoff; cap attempts (e.g., max 2).
- Consider hedging (send a backup after p95 latency) for read-only, idempotent calls.
