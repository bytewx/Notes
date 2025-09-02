# Main Commands

```
# New module
go mod init github.com/bytewx/app && go mod tidy

# Run / Build / Test
go run ./cmd/app
GOFLAGS="-trimpath" go build -ldflags "-s -w -X main.version=$(git rev-parse --short HEAD)" -o bin/app ./cmd/app

# Format / Lint / Vet / Vuln
go fmt ./...
golangci-lint run
go vet ./...
govulncheck ./...

# Bench / Race / Fuzz
go test -race ./...
go test -bench=. -benchmem ./...
go test -fuzz=Fuzz -fuzztime=30s ./...
```
