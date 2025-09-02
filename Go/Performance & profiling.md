# Performance & profiling

```
# Benchmarks
go test -bench=. -benchmem ./...


# CPU/Mem profile (pprof)
go test -run=^$ -bench=BenchmarkFoo -cpuprofile=cpu.out -memprofile=mem.out ./pkg/foo


# Serve pprof in dev
import _ "net/http/pprof" # register handlers
# then: http.ListenAndServe("localhost:6060", nil)
```
- Tips:
- Optimize allocations; keep hot paths on stack, reuse buffers (sync.Pool).
- Avoid interface{} in hot loops; prefer concrete types.
- Measure first; guess last.
