# Makefile (dev UX)

```
APP := app
PKG := ./...
MAIN := ./cmd/app
LD := -s -w -X main.version=$(shell git rev-parse --short HEAD)


.PHONY: run build test lint fmt vet vuln bench tidy
run: ; go run $(MAIN)
build: ; GOFLAGS="-trimpath" go build -ldflags "$(LD)" -o bin/$(APP) $(MAIN)
test: ; go test -race $(PKG)
lint: ; golangci-lint run
fmt: ; go fmt $(PKG)
vet: ; go vet $(PKG)
vuln: ; govulncheck $(PKG)
bench: ; go test -bench=. -benchmem $(PKG)
tidy: ; go mod tidy
```
