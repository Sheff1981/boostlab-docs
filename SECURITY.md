# Security Principles

1. Do not invent a new cryptographic primitive or proprietary encryption protocol.
2. Use the official WireGuard tunnel implementation for the encrypted data path.
3. Never commit private keys, tokens or production credentials.
4. Server WireGuard private keys stay on the gateway.
5. Android WireGuard private keys are generated locally and encrypted at rest with Android Keystore.
6. Only public keys are exchanged for peer registration.
7. Treat gateway configuration received by a client as untrusted until authenticated.
8. Control-plane discovery must use HTTPS; signed configuration is a later hardening step.
9. Keep the control plane out of application payload traffic.
10. Prefer least-privilege processes and non-root services where practical.
11. A failed or unhealthy tunnel must fail safely rather than silently black-hole traffic.
12. Log operational metadata, not user payloads or private key material.
13. Security-sensitive changes require tests before release.
14. Do not expose unauthenticated administrative peer-registration endpoints.
