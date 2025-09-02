# Errors (wrap, inspect, classify)

```
var ErrNotFound = errors.New("not found")


func findUser(ctx context.Context, id int64) (User, error) {
  u, err := repoGet(ctx, id)
  if err != nil {
  if errors.Is(err, sql.ErrNoRows) {
    return User{}, ErrNotFound
  }
    return User{}, fmt.Errorf("get user %d: %w", id, err)
  }
  return u, nil
}
```
- Guidelines:
- Return error as the last value; never panic in libraries.
- Wrap errors with %w; check with errors.Is/As.
- Export stable sentinel/typed errors only when they’re part of your contract.
