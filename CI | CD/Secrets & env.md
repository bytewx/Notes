# Secrets & env

- NEVER commit .env; commit .env.example.
- Put real values in CI variables or a secret store (Vault, SSM, Doppler).
- In Compose, use env_file: .env (synced via rsync from CI).
- In K8s, use Secret + envFrom.
