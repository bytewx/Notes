# GitLab CI (Dockerized app + Nginx on a VPS via SSH)

- Requirements:
- GitLab Variables (Settings → CI/CD → Variables):
- CI_REGISTRY, CI_REGISTRY_USER, CI_REGISTRY_PASSWORD
- PROD_HOST, PROD_USER (e.g. root)
- SSH_PRIVATE_KEY (masked, protected)
- Server has Docker + Compose, and a folder like /srv/app.

- compose.yml (server):
```
services:
  web:
    image: registry.example.com/app/web:${APP_TAG:-latest}
    env_file:
      - .env
    depends_on: [php]     # if PHP-FPM
  php:
    image: registry.example.com/app/php:${APP_TAG:-latest}
  nginx:
    image: nginx:stable
    ports: ["80:80","443:443"]
    volumes:
      - ./nginx/conf.d:/etc/nginx/conf.d:ro
      - ./www:/var/www:ro
    depends_on: [web]
```
- .gitlab-ci.yml:
```
stages: [lint, test, build, push, deploy]

variables:
  DOCKER_DRIVER: overlay2
  APP_IMAGE: "$CI_REGISTRY/app/web"
  APP_TAG: "$CI_COMMIT_SHORT_SHA"
  DOCKER_BUILDKIT: "1"

default:
  image: docker:27
  services: [docker:dind]

before_script:
  - echo "$CI_REGISTRY_PASSWORD" | docker login -u "$CI_REGISTRY_USER" --password-stdin "$CI_REGISTRY"

lint:
  stage: lint
  image: alpine:3.20
  script:
    - echo "run linters here (phpcs/eslint/golangci-lint)"
  rules: [{ if: '$CI_PIPELINE_SOURCE == "push"' }]

test:
  stage: test
  image: alpine:3.20
  script:
    - echo "run tests (phpunit/jest/go test ./...)"
  artifacts:
    when: always
    expire_in: 1 week
    paths: [report.xml]
  rules: [{ if: '$CI_PIPELINE_SOURCE == "push"' }]

build:
  stage: build
  script:
    - docker build -t "$APP_IMAGE:$APP_TAG" .
    - docker tag "$APP_IMAGE:$APP_TAG" "$APP_IMAGE:latest"
  rules:
    - if: '$CI_COMMIT_BRANCH'
  artifacts:
    expire_in: 1 week
    reports: { dotenv: build.env }
  after_script:
    - echo "APP_TAG=$APP_TAG" >> build.env

push:
  stage: push
  needs: ["build"]
  script:
    - docker push "$APP_IMAGE:$APP_TAG"
    - docker push "$APP_IMAGE:latest"
  rules:
    - if: '$CI_COMMIT_BRANCH'

deploy_prod:
  stage: deploy
  image: alpine:3.20
  needs: ["push"]
  variables:
    GIT_SSH_COMMAND: 'ssh -o StrictHostKeyChecking=no'
  before_script:
    - apk add --no-cache openssh-client rsync
    - mkdir -p ~/.ssh && chmod 700 ~/.ssh
    - echo "$SSH_PRIVATE_KEY" | tr -d '\r' | ssh-add - >/dev/null 2>&1 || eval $(ssh-agent) && ssh-add -
    - ssh-keyscan -H "$PROD_HOST" >> ~/.ssh/known_hosts
  script:
    # sync configs (compose, nginx, env)
    - rsync -az --delete ./compose.yml ./nginx/ "$PROD_USER@$PROD_HOST:/srv/app/"
    - ssh "$PROD_USER@$PROD_HOST" "cd /srv/app && APP_TAG=$APP_TAG docker compose pull && APP_TAG=$APP_TAG docker compose up -d"
    # validate & hot-reload nginx
    - ssh "$PROD_USER@$PROD_HOST" "docker exec nginx nginx -t && docker exec nginx nginx -s reload"
  environment:
    name: production
    url: https://app.example.com
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
      when: manual
      allow_failure: false
```
- Notes:
- Manual prod deploy protects you from accidental pushes.
- For zero downtime, either:
- use Compose to replace containers (nginx stays up), or
- run blue/green (see below) and switch upstreams.
