# Security Principles

1. Do not invent a new cryptographic primitive.
2. Never commit private keys, tokens or production credentials.
3. Treat gateway configuration received by a client as untrusted until authenticated.
4. Keep the control plane out of application payload traffic.
5. Prefer least-privilege processes and non-root containers where practical.
6. A failed or unhealthy tunnel must fail safely rather than silently black-hole traffic.
7. Log operational metadata, not user payloads.
8. Security-sensitive changes require tests before release.
