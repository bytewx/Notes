# HTTP server (graceful, timeouts, healthz)

```
// cmd/app/main.go
package main

import (
  "context"
  "errors"
  "log/slog"
  "net/http"
  "os"
  "os/signal"
  "syscall"
  "time"
  
  
  "github.com/go-chi/chi/v5"
  "github.com/go-chi/chi/v5/middleware"
  "github.com/bytewx/app/internal/config"
  "github.com/bytewx/app/internal/logx"
)


var version = "dev" // overridden by -ldflags


func main() {
cfg := config.FromEnv()
log := logx.New(cfg.LogLevel).With("version", version)


r := chi.NewRouter()
r.Use(middleware.RequestID, middleware.RealIP)
r.Use(middleware.Recoverer)
r.Use(middleware.Timeout(30 * time.Second))


r.Get("/healthz", func(w http.ResponseWriter, r *http.Request) { w.WriteHeader(http.StatusOK); _, _ = w.Write([]byte("ok")) })
r.Get("/hello", hello)


srv := &http.Server{
  Addr: cfg.Addr,
  Handler: r,
  ReadHeaderTimeout: 5 * time.Second,
  ReadTimeout: cfg.ReadTimeout,
  WriteTimeout: cfg.WriteTimeout,
  IdleTimeout: cfg.IdleTimeout,
}


ctx, stop := signal.NotifyContext(context.Background(), syscall.SIGINT, syscall.SIGTERM)
defer stop()


go func() {
  log.Info("http listening", "addr", cfg.Addr)
  if err := srv.ListenAndServe(); !errors.Is(err, http.ErrServerClosed) {
    log.Error("server error", "err", err)
    os.Exit(1)
  }
  }()
  
  
  <-ctx.Done()
  log.Info("shutting down")
  shutdownCtx, cancel := context.WithTimeout(context.Background(), 10*time.Second)
  defer cancel()
  if err := srv.Shutdown(shutdownCtx); err != nil { log.Error("graceful shutdown failed", "err", err) }
}


func hello(w http.ResponseWriter, r *http.Request) {
  w.Header().Set("Content-Type", "application/json")
  w.WriteHeader(http.StatusOK)
  _, _ = w.Write([]byte(`{"message":"hello"}`))
}
```
