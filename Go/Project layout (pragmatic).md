# Project layout (pragmatic)

```
.
├─ cmd/app/ # main() entry
│ └─ main.go
├─ internal/ # private packages (not importable outside module)
│ ├─ http/ # handlers, middleware
│ ├─ service/ # business logic
│ ├─ repo/ # db adapters
│ └─ config/
├─ pkg/ # optional: public, importable packages
├─ api/ # OpenAPI/Protos, generated clients/servers
├─ migrations/ # SQL migrations (migrate/goose)
├─ scripts/ # dev tools
├─ web/ # static assets (embed-able)
├─ .golangci.yml
├─ Dockerfile
└─ Makefile
```
- Rule of thumb: start with cmd/ + internal/. Introduce pkg/ only when you have reusable lib code.
