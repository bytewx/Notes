# Common pitfalls

- Forgetting to close response bodies (defer resp.Body.Close()).
- Capturing loop variable in goroutine — copy i := i before launching.
- Ignoring context cancellation in long ops.
- Leaking tickers/timers (always Stop).
- Overusing interfaces: export concrete types unless abstraction is needed.
