# BOOSTLAB Documentation

Architecture, protocol decisions, deployment notes and development roadmap for the BOOSTLAB project.

## Repositories

- `boostlab-android` — Android client.
- `boostlab-gateway` — Linux data-plane gateway.
- `boostlab-control` — control-plane API.
- `boostlab-windows` — future Windows client.
- `boostlab-docs` — shared architecture and operational documentation.

## Engineering rule

A feature is not called a “boost” unless measurements show an actual improvement in latency stability, packet loss, routing quality, or connection reliability. The client must always be able to fall back safely to the normal network path.
