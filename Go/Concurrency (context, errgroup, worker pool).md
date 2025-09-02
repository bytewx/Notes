# Concurrency (context, errgroup, worker pool)

```
// Parallel tasks with cancellation
import "golang.org/x/sync/errgroup"


eg, ctx := errgroup.WithContext(r.Context())
for i := 0; i < 3; i++ {
  i := i
  eg.Go(func() error { return job(ctx, i) })
}
if err := eg.Wait(); err != nil { return err }

// Worker pool pattern
func Pool[T any](ctx context.Context, work <-chan T, n int, fn func(context.Context, T) error) error {
  g, ctx := errgroup.WithContext(ctx)
    for i := 0; i < n; i++ {
      g.Go(func() error {
        for {
          select {
          case <-ctx.Done():
          return ctx.Err()
          case w, ok := <-work:
          if !ok { return nil }
          if err := fn(ctx, w); err != nil { return err }
          }
        }
      })
    }
  return g.Wait()
}
```
- Rules:
- Always pass context.Context first param.
- Stop Ticker/Timer with defer t.Stop().
- Close channels from the sender side.
- Prefer errgroup to manual wg+chan error plumbing.
