# Security basics

- Always validate inputs; for JSON: Decoder.DisallowUnknownFields().
- Timeouts everywhere (client & server). Avoid outbound calls without context deadlines.
- Use crypto/rand for tokens; bcrypt/argon2id for passwords.
- Do not embed secrets; use env/secret store. Rotate keys.
- Serve HTTPS (behind Nginx or directly with http.Server + TLS).
