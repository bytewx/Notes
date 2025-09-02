# Repo layout (example)

```
.
├─ app/                 # your app (php / node / go)
├─ nginx/               # nginx configs (conf.d/, snippets/)
├─ compose.yml          # prod docker compose
├─ compose.override.yml # dev overrides (ignored in CI)
├─ .env.example         # env template
├─ .gitlab-ci.yml       # GitLab CI pipeline
└─ deploy/              # scripts, ansible, k8s, etc.
```
