# HTTP middleware (rate limit example)

```
func Rate(limit int, per time.Duration) func(http.Handler) http.Handler {
  type bucket struct{ tokens int; last time.Time }
  b := &bucket{tokens: limit, last: time.Now()}
  mu := &sync.Mutex{}
  
  refill := func() { // naive token bucket
  now := time.Now()
  elapsed := now.Sub(b.last)
  add := int(elapsed / (per / time.Duration(limit)))
  if add > 0 {
    if b.tokens+add > limit { b.tokens = limit } else { b.tokens += add }
    b.last = now
    }
  }
  return func(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
      mu.Lock(); refill()
      if b.tokens == 0 { mu.Unlock(); http.Error(w, "rate limit", 429); return }
      b.tokens--; mu.Unlock()
      next.ServeHTTP(w, r)
      })
    }  
}
```
