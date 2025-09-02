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
