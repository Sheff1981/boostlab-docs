# BOOSTLAB Technology Policy

BOOSTLAB uses current stable technology with a bias toward security, maintainability and native platform support.

## Client applications

### Android

- Language: Kotlin.
- UI: Jetpack Compose.
- Async/state: Kotlin coroutines + StateFlow.
- VPN/tunnel: official WireGuard Android tunnel library.
- Secure local key storage: Android Keystore.
- Build: Gradle Kotlin DSL.

Not part of the target architecture:

- Java-first Android application code.
- XML-first UI for new screens.
- custom cryptography.

### Windows

Windows client work is **paused**. The current product target is Android only.

The existing C# / .NET 10 / WinUI 3 repository is retained so work is not lost, but no new Windows-client features should be implemented until Android is proven end-to-end.

Windows can still act as a temporary local development gateway for no-VPS LAN tests.

## Server side

### Gateway

- Language: Go.
- Encrypted data plane: standard WireGuard on Linux.
- Probe/health service: Go.
- Firewall/NAT: nftables.
- Service supervision: systemd.
- Container option: OCI/Docker image.

### Control plane

- Language: Go.
- Transport: HTTPS/JSON.
- Responsibilities: node discovery, health metadata and future authenticated orchestration.
- User application payload traffic never belongs in the control plane.

## Engineering rules

1. Prefer current stable releases over preview or experimental releases.
2. Upgrade deliberately and keep CI green.
3. Do not rewrite a working subsystem merely to chase a fashionable framework.
4. Use platform-native security facilities.
5. Do not invent cryptographic protocols.
6. Separate control plane from data plane.
7. New code must be testable and observable.
8. CI must build every supported client/server target before release.
