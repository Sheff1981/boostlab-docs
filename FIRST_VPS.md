# BOOSTLAB First VPS Runbook

This is the first real end-to-end test target. It does not require multiple regions yet.

## Services on one Linux VPS

- WireGuard data plane: UDP 51820.
- BOOSTLAB route probe: UDP 51821.
- BOOSTLAB gateway health: TCP 8080, preferably private/firewalled.
- BOOSTLAB control API: TCP 8090 behind HTTPS.
- HTTPS reverse proxy: TCP 443.

## Deployment order

1. Provision a small current Linux VPS with a public IP.
2. Install WireGuard and nftables from the distribution packages.
3. Build/download the green GitHub artifacts:
   - `boostlab-gateway-linux-amd64`
   - `boostlab-peerctl-linux-amd64`
   - `boostlab-control-linux-amd64`
4. Generate the WireGuard server key **on the VPS**.
5. Configure `wg0` with `10.77.0.1/24`, UDP 51820 and nftables forwarding/NAT.
6. Set `BOOSTLAB_WG_PUBLIC_KEY` to the server **public** key in the gateway environment.
7. Start the BOOSTLAB gateway probe service.
8. Start the control service and publish the gateway host, probe port, WireGuard public key and WireGuard port.
9. Put the control service behind HTTPS.
10. Open provider/firewall ingress only as needed:
    - TCP 22 from the administrator's IP where possible;
    - TCP 443 public;
    - UDP 51820 public;
    - UDP 51821 public.
11. Keep TCP 8080 and 8090 private/local when the HTTPS proxy is in place.

## First Android peer

1. Install the current BOOSTLAB Android debug build.
2. Copy the **public** key shown by the app.
3. On the VPS, dry-run:

       boostlab-peerctl --public-key '<ANDROID_PUBLIC_KEY>' --address 10.77.0.2/32

4. Apply after checking the values:

       sudo boostlab-peerctl --public-key '<ANDROID_PUBLIC_KEY>' --address 10.77.0.2/32 --apply

5. Persist the peer in the protected WireGuard configuration.
6. In Android:
   - select an application;
   - measure the gateway;
   - enter/use the server public key;
   - keep the assigned address `10.77.0.2/32`;
   - connect.

The Android client now refuses to report the boost as active until a fresh WireGuard handshake is observed. If the handshake fails, it closes the tunnel so the selected application can return to the normal network path.

## First acceptance test

The first VPS test is successful only when all of the following are proven:

- UDP 51821 probe returns stable RTT/jitter/loss values.
- WireGuard handshake succeeds.
- Only the selected Android application is routed through WireGuard.
- Other phone applications retain their normal network route.
- Disconnect restores the selected application to the normal route.
- A broken/unreachable WireGuard server causes safe fallback rather than a persistent black hole.
- Direct-route metrics and gateway-route metrics are recorded for comparison.

Do not call a route a “boost” merely because the tunnel connected. The boosted path must be measurably better or more stable for the target connection.
