# BOOSTLAB Monetization Model

BOOSTLAB should be usable for free while keeping the network infrastructure economically sustainable.

## Free tier

The free tier is the default.

Planned behavior:

- access to the production gateway network;
- route measurement and automatic gateway selection;
- ads enabled;
- no ad overlay while an active boost session is running;
- no sale of application payloads or private traffic data;
- operational telemetry only where required for routing, abuse prevention and reliability.

Advertising is intended to offset gateway, bandwidth, monitoring and support costs.

## Premium tier

Planned behavior:

- no ads;
- access to future priority-routing/capacity features;
- the same privacy boundary as the free tier;
- entitlement verified by the control plane before production release.

Premium is not allowed to disable security checks or safe fallback behavior.

## Store architecture

BOOSTLAB should not couple the networking core to one advertising or billing vendor.

The Android application keeps these layers separate:

1. Networking core — route probes, selection and WireGuard.
2. Entitlements — Free/Premium state.
3. Advertising adapter — store/distribution-specific implementation.
4. Billing adapter — store/distribution-specific implementation.

This allows separate Play Market and RuStore release integrations without duplicating the network engine.

## Advertising rules

- Never show an interstitial over a game or while the tunnel is active.
- Do not delay emergency disconnect/fallback to display an ad.
- Ads belong on home, results, server-selection or other idle UI.
- The debug/development build uses a placeholder/no-op surface, not a production ad network.
- Consent/privacy requirements must be implemented before enabling production ads.

## Infrastructure economics

A free application is not a serverless application.

Expected cost centers:

- Linux gateways/VPS;
- network bandwidth/traffic;
- public IP addresses;
- monitoring/logging;
- control-plane hosting;
- support and abuse handling.

The first engineering objective is not maximizing ad impressions. It is proving that one inexpensive gateway can provide a measurably useful route for real users.

## Current implementation

- Control plane exposes `GET /v1/client-policy`.
- Default tier is Free.
- Free policy currently enables ads but does not intentionally cripple route quality during testing.
- Premium policy disables ads and reserves a priority-routing entitlement for later.
- Android consumes the policy and shows the current tier.
- Advertising is hidden while a boost session is active.
- No production advertising SDK or payment SDK is embedded yet.
