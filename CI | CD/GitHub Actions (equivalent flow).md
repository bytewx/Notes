# GitHub Actions (equivalent flow)

- .github/workflows/ci.yml:
```
name: CI
on:
  push:
    branches: [main, develop]
  pull_request:
jobs:
  build-test-push:
    runs-on: ubuntu-latest
    permissions: { contents: read, packages: write }
    steps:
      - uses: actions/checkout@v4
      - uses: docker/setup-buildx-action@v3
      - uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
      - name: Build & push
        run: |
          IMAGE=ghcr.io/${{ github.repository }}/web
          TAG=${GITHUB_SHA::7}
          docker build -t $IMAGE:$TAG .
          docker tag $IMAGE:$TAG $IMAGE:latest
          docker push $IMAGE:$TAG
          docker push $IMAGE:latest

  deploy-prod:
    needs: build-test-push
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup SSH
        run: |
          mkdir -p ~/.ssh
          echo "${{ secrets.SSH_KEY }}" > ~/.ssh/id_ed25519
          chmod 600 ~/.ssh/id_ed25519
          ssh-keyscan -H ${{ secrets.PROD_HOST }} >> ~/.ssh/known_hosts
      - name: Rsync files & deploy
        run: |
          rsync -az --delete compose.yml nginx/ ${{ secrets.PROD_USER }}@${{ secrets.PROD_HOST }}:/srv/app/
          ssh ${{ secrets.PROD_USER }}@${{ secrets.PROD_HOST }} \
            "cd /srv/app && APP_TAG=${GITHUB_SHA::7} docker compose pull && APP_TAG=${GITHUB_SHA::7} docker compose up -d && docker exec nginx nginx -t && docker exec nginx nginx -s reload"
```
