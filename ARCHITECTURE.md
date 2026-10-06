# BOOSTLAB Architecture

## Goal

BOOSTLAB is a per-application network routing system. It should improve route quality only when an alternate path is measurably better than the device's normal path.

## Components

### Android client

- Discovers launchable apps.
- Lets the user choose the app to route.
- Requests VPN permission with Android VpnService.
- Measures candidate gateways.
- Gets a healthy gateway list from the HTTPS control plane.
- Scores routes using median RTT, jitter and packet loss.
- Avoids route flapping unless a candidate is materially better.
- Uses the official embeddable WireGuard Android tunnel library for the encrypted data path.
- Restricts the tunnel to the selected package using WireGuard `IncludedApplications`.
- Keeps the client WireGuard private key encrypted at rest with an Android Keystore AES-GCM wrapping key.
- Falls back to the normal network path if the tunnel is not established.

### Gateway

The gateway has two responsibilities that remain separate:

1. BOOSTLAB probe service:
   - HTTP health/status.
   - UDP 51821 route-quality probes.
2. Standard WireGuard data plane:
   - UDP 51820.
   - Linux WireGuard interface.
   - Forwarding/NAT to the public network.

BOOSTLAB does not invent a custom cryptographic tunnel protocol.

### Control plane

The control plane carries metadata and orchestration:

- gateway list;
- region/server discovery;
- health state;
- future authenticated client registration and entitlements.

It does not carry or inspect game/application payload traffic.

### Windows client

Reserved for the second client implementation after the Android data path is proven.

## Route selection

A route score considers:

1. median RTT;
2. jitter;
3. packet loss;
4. route availability.

A gateway does not win merely because one probe returned the lowest ping.

When the app already has a selected gateway, a new node must be materially better before the client switches. This reduces route flapping.

## Key model

- Server WireGuard private key is generated and stored only on the gateway.
- Android client WireGuard private key is generated locally.
- The client private key is encrypted at rest using Android Keystore.
- Only public keys are exchanged for peer registration.
- Private keys must never enter Git, logs, screenshots, or the public control-plane node list.

## Privacy

The system should collect only operational telemetry required to choose and maintain a route. Game/application payload contents are not a control-plane concern.
