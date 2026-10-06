# BOOSTLAB Network Protocols

## Route-quality probe v1

Default gateway probe port: **UDP 51821**

Client sends the exact UTF-8 payload:

    BOOSTLAB/PROBE/1

A healthy gateway returns the same payload to the sender.

Repeated samples allow a client to calculate:

- median round-trip time;
- packet loss;
- jitter;
- route stability.

This is a measurement protocol only. It carries no application traffic.

Unknown UDP payloads are ignored. The response is intentionally tiny to avoid creating a useful amplification service.

## Encrypted data plane

Default tunnel port: **UDP 51820**

The encrypted application data path uses standard WireGuard. BOOSTLAB does not define its own cryptographic packet format.

For Android, the WireGuard interface configuration includes only the user-selected package through `IncludedApplications`, while peer `AllowedIPs` covers the destination address space transported through the tunnel.

## Control plane

Gateway discovery is exposed by:

    GET /v1/nodes

The Android client requires an HTTPS base URL for automatic discovery.

Node objects currently contain:

- `id`
- `region`
- `host`
- `udp_port`
- `healthy`
- `updated_at`

The control-plane node list contains routing metadata only. It must never contain WireGuard private keys.
