# Testing (table-driven, http, golden, fuzz)

```
func TestAdd(t *testing.T) {
  t.Parallel()
  cases := []struct{ a, b, want int }{{1,2,3},{-1,1,0}}
  for _, c := range cases {
    got := Add(c.a, c.b)
    if got != c.want { t.Fatalf("Add(%d,%d)=%d want %d", c.a,c.b,got,c.want) }
  }
}

// HTTP
func TestHello(t *testing.T) {
  r := chi.NewRouter()
  r.Get("/hello", hello)
  req := httptest.NewRequest(http.MethodGet, "/hello", nil)
  w := httptest.NewRecorder()
  r.ServeHTTP(w, req)
  if w.Code != 200 { t.Fatal(w.Code, w.Body.String()) }
}

// Fuzz (Go ≥1.18)
func FuzzParse(f *testing.F) {
  f.Add("2025-09-01")
  f.Fuzz(func(t *testing.T, s string) { _ = ParseDate(s) })
}
```
- Tools: testcontainers-go for integration; mockgen or interfaces for edges.
