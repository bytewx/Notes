# Questions & Talking Points

- Image vs Container: immutable template vs runtime instance.
- Layer caching: order matters; lockfiles first.
- ARG vs ENV: build-time vs runtime; security implications.
- Bind mount vs volume: dev live-reload vs persistent data.
- Multi-stage: smaller, safer images; separate build/run concerns.
- Healthcheck: readiness/liveness signaling.
- Non-root: security, capabilities, read-only FS.
- Networking: user-defined networks, service discovery by name.
- Compose: orchestration for local/dev; parallels to K8s manifests.
- Debugging: logs, exec, inspect, top, stats.
- Reproducibility: pin tags, lockfiles, CI builds, SBOM/scans.
- Registry auth & tagging: semver tags, immutable digests.
