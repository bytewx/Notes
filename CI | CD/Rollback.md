# Rollback

- Keep a small release registry (tags or a releases.txt artifact).
- To roll back:
1) choose previous tag,
2) set APP_TAG=<prev> and redeploy,
3) docker exec nginx nginx -s reload if needed.
