# Linting (golangci-lint)

```
# .golangci.yml
run:
  timeout: 5m
linters:
  enable:
  - govet
  - staticcheck
  - gofmt
  - goimports
  - revive
  - errcheck
  - ineffassign
  - misspell
  - gosimple
issues:
  exclude-rules:
    - path: _test\.go
    linters: [revive]
```
