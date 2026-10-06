# BOOSTLAB Roadmap

## Current product target

BOOSTLAB is currently an **Android-only private build for one friend**.

- Android client is the only active client target.
- Windows client development is paused.
- Windows may still be used as a temporary local development gateway during no-VPS testing.
- Production networking work remains Android + Linux gateway/control plane.

## Stage 1 — Android shell

- [x] Compose application shell.
- [x] Installed-app discovery.
- [x] App selection.
- [x] Android VPN permission flow.
- [x] Foreground VPN service shell.
- [x] CI baseline.

## Stage 2 — First measurable gateway

- [x] Gateway repository.
- [x] HTTP health/status.
- [x] UDP probe service.
- [x] Gateway CI and container build definition.
- [ ] Deploy one Linux gateway.
- [x] Android UDP probe client.
- [x] Display live RTT, jitter and packet loss.

## Stage 3 — Encrypted per-app tunnel

- [ ] Choose proven tunnel implementation.
- [ ] Authenticate gateway configuration.
- [ ] Route only the selected Android app.
- [ ] Safe tunnel teardown and fallback.
- [ ] IPv4/IPv6 and DNS behavior tests.

## Stage 4 — Automatic route selection

- [x] Multiple regions/nodes.
- [x] Initial quality score using RTT/jitter/loss.
- [ ] Automatic node switching with hysteresis.
- [x] Control-plane node discovery.

## Stage 5 — Production hardening

- [ ] Abuse/rate controls.
- [ ] Metrics and alerting.
- [ ] Signed client configuration.
- [ ] Release signing.
- [ ] Privacy/retention policy.
- [ ] Failure-injection and load tests.

## Later

- Windows client — paused; current target is Android only.
- Accounts/subscriptions if required later.
- Regional capacity scaling.


## Stage 3 discovery groundwork

- [x] Control service can load a deterministic gateway list from `BOOSTLAB_NODES_JSON`.
- [x] Android can fetch healthy gateways over HTTPS.
- [x] Android benchmarks several gateways in parallel.
- [x] Route score penalizes jitter and packet loss, not only ping.
- [x] Add switching hysteresis; rolling history still follows real multi-node measurements.


## Stage 4 — WireGuard groundwork

- [x] Select standard WireGuard instead of inventing a custom encrypted tunnel.
- [x] Add the official Android WireGuard tunnel library.
- [x] Add per-app WireGuard config generation using `IncludedApplications`.
- [x] Generate the Android client identity locally.
- [x] Encrypt the client private key at rest with Android Keystore AES-GCM.
- [x] Add Linux WireGuard gateway configuration examples.
- [x] Add nftables forwarding/NAT baseline.
- [ ] Deploy the first real gateway and register the first Android public key.
- [x] Wire Android to the real WireGuard backend; live handshake awaits the first VPS.
- [x] Configure Android WireGuard with IncludedApplications; live traffic verification awaits the first VPS.
- [ ] Measure direct route vs boosted route under real network conditions.


## Local no-VPS development path

- [x] Build a Windows version of the BOOSTLAB probe gateway.
- [x] Package a Windows local-test bundle with PowerShell start/firewall helpers.
- [x] Add Android LAN discovery over the existing UDP probe protocol.
- [x] Benchmark discovered LAN gateways automatically.
- [ ] Verify discovery on two physical devices on the same Wi-Fi.
- [ ] Verify the first real remote WireGuard route after a VPS or other remotely reachable gateway is available.
