# Release gates & environments

- Protect main branch; require green tests before deploy.
- Use GitLab environments (staging, production) with manual when: manual.
- Add review apps: per-MR ephemeral environment (docker compose -p pr-<iid>), auto-destroy on close.
