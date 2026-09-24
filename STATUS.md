# OpenTouch — Project Status

**Last updated:** 2026-09-24  
**Current phase:** Phase 0A — Raw-sensor feasibility  
**Architecture:** Raw commercial fingerprint sensor + OpenTouch controller/PCB  
**Current action:** OT-0001 — Raw Sensor Feasibility Scan  
**Status:** IN PROGRESS — FPC RESPONSE PENDING

## Current state

Fingerprint Cards is the leading raw-sensor candidate.

Current classification:

```text
FPC:
OPEN_DEV_CANDIDATE
DOCUMENTATION_UNRESOLVED
VENDOR_RESPONSE_PENDING
```

A focused inquiry was sent to FPC on 2026-09-24 asking for:

- the current recommended raw sensor;
- direct raw/grayscale frame availability;
- low-level programming documentation;
- NDA requirements;
- permission to publish an independent open-source driver;
- correct development kit;
- sample/MOQ/lifecycle/pricing information;
- separation between hardware liveness capabilities and licensed algorithm features.

## Current authoritative records

- Architecture: `docs/decisions/ADR-0001-raw-sensor-opentouch-controller.md`
- Active action: `docs/actions/OT-0001-raw-sensor-feasibility-scan.md`
- Public research pass: `docs/research/OT-0001-pass-01-public-raw-sensor-scan.md`
- Vendor inquiry: `docs/research/OT-0001-fpc-vendor-inquiry-2026-09-24.md`

## Hardware purchasing status

**WAIT on FPC raw-sensor hardware.**

Do not buy generic `FPC1020A` UART/USB fingerprint modules as the development platform. Many include a separate controller and built-in enrollment/matching stack, which bypasses the raw sensor interface OpenTouch intends to own.

The raw FPC purchase should follow the vendor's answer or an independently verified bare-sensor/dev-board path.

## Next decision

If FPC confirms a publishable low-level integration path:

> ADR-0002 — Select OpenTouch Development Fingerprint Sensor

If FPC rejects or materially restricts that path, OT-0001 continues with alternative vendors.
