# BOOSTLAB Architecture

## Goal

BOOSTLAB is a per-application network routing system. It should improve route quality only when an alternate path is measurably better than the device's normal path.

## Components

### Android client

- Discovers launchable apps.
- Lets the user choose the app to route.
- Requests VPN permission with Android VpnService.
- Measures candidate gateways.
- Establishes the tunnel only after a healthy gateway is available.
- Routes only the selected application's traffic.
- Falls back to the normal network path if the tunnel becomes unhealthy.

### Gateway

The gateway is the data plane.

Current baseline:

- TCP health/status endpoint.
- UDP probe endpoint for real RTT/loss/jitter sampling.
- Region and node identity from environment configuration.

The gateway does not yet relay user traffic. Traffic tunnelling is a later stage and will use established cryptographic/networking primitives rather than custom cryptography.

### Control plane

The control plane is metadata and orchestration:

- gateway registry;
- health state;
- region/server discovery;
- future authentication and entitlements.

It is intentionally separate from user payload traffic.

### Windows client

Reserved for the second client implementation after the Android data path is proven.

## Selection model

A future client score should consider more than raw ping:

1. median RTT;
2. jitter;
3. packet loss;
4. recent gateway health;
5. route stability over a rolling window.

A gateway should not be selected merely because one probe returned the lowest number.

## Privacy

The system should collect only operational telemetry required to choose and maintain a route. Game/application payload contents are not a control-plane concern.
