# Probe Protocol v1

This document describes the temporary route-quality probe used before the real tunnel exists.

## UDP endpoint

Default gateway UDP port: **51821**

Client sends the exact UTF-8 payload:

    BOOSTLAB/PROBE/1

A healthy gateway returns the same payload to the sender.

## Purpose

Repeated samples allow a client to calculate:

- round-trip time;
- packet loss;
- jitter/variance;
- route stability.

This is a measurement protocol only. It carries no application traffic and is not the future tunnel protocol.

## Safety

Unknown UDP payloads are ignored. The probe response is intentionally tiny to avoid turning the service into a meaningful amplification mechanism.
