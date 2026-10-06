# BOOSTLAB Network Protocols

## Route-quality probe

Default gateway probe port: **UDP 51821**

### v1 — LAN discovery

The v1 payload remains intentionally simple:

    BOOSTLAB/PROBE/1

The gateway echoes the same bytes.

BOOSTLAB Android can broadcast this packet on the local network to discover a nearby development gateway without a VPS. The source IP of each valid reply identifies the local gateway.

### v2 — correlated measurements

Normal RTT/jitter/loss measurements use a unique payload for every sample:

    BOOSTLAB/PROBE/2/<nonce>/<sequence>

Example:

    BOOSTLAB/PROBE/2/0123456789abcdef/7

The gateway validates the format and echoes the **exact same bytes**.

The nonce and sequence number prevent a late UDP reply from an older sample being counted as the current sample. This matters when calculating packet loss and jitter on unstable networks.

Current constraints:

- nonce: 16 lowercase hexadecimal characters;
- sequence: decimal 0..999;
- maximum request/response size: 96 bytes;
- response size never exceeds request size.

Repeated samples allow a client to calculate:

- median round-trip time;
- packet loss;
- jitter;
- route stability.

The probe carries no application payload traffic.

Unknown or malformed UDP payloads are ignored.

## Encrypted data plane

Default tunnel port: **UDP 51820**

The encrypted application data path uses standard WireGuard. BOOSTLAB does not define its own cryptographic packet format.

For Android, the WireGuard interface configuration includes only the user-selected package through `IncludedApplications`, while peer `AllowedIPs` covers the destination address space transported through the tunnel.

The Android client requires a fresh WireGuard handshake before it reports the boost as active. A failed handshake causes the tunnel to close so normal connectivity can recover.

## Control plane

Gateway discovery is exposed by:

    GET /v1/nodes

The Android client requires an HTTPS base URL for remote automatic discovery.

Node objects currently contain:

- `id`
- `region`
- `host`
- `udp_port`
- `wireguard_public_key` when configured
- `wireguard_port` when configured
- `healthy`
- `updated_at`

The control-plane node list contains public routing metadata only. It must never contain WireGuard private keys.
