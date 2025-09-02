# Database (Postgres via pgx, migrations)

```
// internal/repo/db.go
package repo


import (
"context"
"github.com/jackc/pgx/v5/pgxpool"
)


type DB struct{ Pool *pgxpool.Pool }


func Connect(ctx context.Context, url string) (*DB, error) {
  p, err := pgxpool.New(ctx, url)
  if err != nil { return nil, err }
  return &DB{Pool: p}, nil
}


func (db *DB) Close() { db.Pool.Close() }
```
- Migrations (choose one):
```
# golang-migrate
migrate -path migrations -database $DATABASE_URL up
# or goose
goose -dir ./migrations postgres "$DATABASE_URL" up
```
- Best practices:
- Keep queries in SQL, generate code w/ sqlc for type safety.
- Use pooling defaults, tune MaxConns, timeouts, statement timeouts server-side.
- Treat DB URL as a secret (env/CI secret store).
