# Useful snippets

- Cache dependencies in CI (example Node):
```
cache:
  key: ${CI_COMMIT_REF_SLUG}
  paths: [node_modules/]
```
- Conditional deploy:
```
rules:
  - if: '$CI_COMMIT_TAG'         # deploy only on tags
```
- Artifacts:
```
artifacts:
  paths: [dist/, coverage/]
  expire_in: 7 days
```
