# Config (flags + env) — minimal, testable

```
// internal/config/config.go
package config


import (
  "flag"
  "os"
  "time"
)


type Config struct {
  Addr string // ":8080"
  DBURL string // "postgres://..."
  ReadTimeout time.Duration // 10s
  WriteTimeout time.Duration // 10s
  IdleTimeout time.Duration // 60s
  LogLevel string // "info|debug|warn|error"
}


func FromEnv() Config {
  var c Config
  flag.StringVar(&c.Addr, "addr", getenv("ADDR", ":8080"), "http listen address")
  flag.StringVar(&c.DBURL, "db", getenv("DATABASE_URL", ""), "postgres url")
  flag.DurationVar(&c.ReadTimeout, "read-timeout", 10*time.Second, "read timeout")
  flag.DurationVar(&c.WriteTimeout, "write-timeout", 10*time.Second, "write timeout")
  flag.DurationVar(&c.IdleTimeout, 60*time.Second, 60*time.Second, "idle timeout")
  flag.StringVar(&c.LogLevel, "log-level", getenv("LOG_LEVEL", "info"), "log level")
  flag.Parse()
  return c
}


func getenv(k, def string) string {
  if v := os.Getenv(k); v != "" { return v }
  return def
}
```
