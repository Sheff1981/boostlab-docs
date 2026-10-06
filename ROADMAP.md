# BOOSTLAB Roadmap

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

- Windows client.
- Accounts/subscriptions if required.
- Regional capacity scaling.


## Stage 3 discovery groundwork

- [x] Control service can load a deterministic gateway list from `BOOSTLAB_NODES_JSON`.
- [x] Android can fetch healthy gateways over HTTPS.
- [x] Android benchmarks several gateways in parallel.
- [x] Route score penalizes jitter and packet loss, not only ping.
- [ ] Add rolling history and switching hysteresis after real multi-node measurements exist.
