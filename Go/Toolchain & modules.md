# Toolchain & modules

- Keep Go ≥1.21 (slog, toolchain directive) or Go 1.22 if you need the newer for/range semantics.
- go.mod:
```
module github.com/bytewx/app


go 1.22


// optional: pin toolchain to help collaborators
// toolchain go1.22.5


require (
  github.com/go-chi/chi/v5 v5.0.11
  github.com/jackc/pgx/v5 v5.5.0
  golang.org/x/sync v0.8.0
)
```
- Use replace only for local dev (replace module => ../path). Remove before pushing.
- Monorepo? Use go work:
```
go work init ./service-a ./service-b
```
