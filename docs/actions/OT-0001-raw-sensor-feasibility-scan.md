# OT-0001 — Raw Sensor Feasibility Scan

- **Status:** IN PROGRESS — VENDOR RESPONSE PENDING
- **Priority:** P0
- **Phase:** Phase 0A — Raw-sensor feasibility
- **Opened:** 2026-09-24
- **Closed:** —
- **Related ADRs:** ADR-0001 — Adopt Raw-Sensor / OpenTouch-Controller Architecture
- **Depends on:** none
- **Blocks:** development sensor selection; controller selection; first raw-sensor hardware prototype

## Objective

Determine whether at least one commercially available fingerprint sensing IC/module gives OpenTouch enough technical, legal, and commercial freedom to build an open controller/firmware/Linux stack around it.

## Current Findings

### Fingerprint Cards

Current classification:

```text
OPEN_DEV_CANDIDATE
DOCUMENTATION_UNRESOLVED
VENDOR_RESPONSE_PENDING
```

Public evidence establishes:

- FPC sells raw sensor products intended for customer MCU/memory integration;
- FPC licenses its biometric algorithm separately;
- historical FPC1020 GPL code demonstrates direct low-level SPI image acquisition;
- mainstream distribution/support infrastructure exists.

Public evidence does **not** establish:

- publication rights for a driver targeting the current recommended FPC10/FPC15 part;
- whether current full programming documentation is NDA-bound;
- whether current raw acquisition requires any binary component.

A focused vendor inquiry was sent on 2026-09-24.

See:

[`../research/OT-0001-fpc-vendor-inquiry-2026-09-24.md`](../research/OT-0001-fpc-vendor-inquiry-2026-09-24.md)

### Goodix

Current classification:

```text
DOCUMENTATION_BLOCKED
CLOSED_VENDOR_STACK_RISK
```

Goodix remains secondary pending evidence of a legally publishable low-level integration path.

## Current Gate

Await FPC's response to the low-level documentation/open-source publication questions.

The highest-value answers are:

- current recommended raw sensor;
- raw-image access;
- register/interface documentation;
- NDA requirements;
- permission to publish an independent driver;
- correct raw-sensor dev kit;
- lifecycle/MOQ/pricing.

## Purchasing Status

**Do not purchase a generic FPC1020A UART/USB module as OpenTouch development hardware.**

Many currently available retail modules add an onboard controller and biometric algorithm. They expose a high-level command interface and therefore do not validate the raw-sensor architecture.

Purchase of a raw FPC development platform is blocked until:

- FPC identifies the appropriate current sensor/dev kit, or
- an independently verified bare-sensor platform is found with appropriate documentation.

A separate benchmark reader may still be purchased later if its exact hardware identity is verified.

## Next Work

While the FPC response is pending:

1. preserve the vendor-response gate;
2. perform only targeted second-source research;
3. do not choose an MCU or PCB;
4. do not accept ADR-0002;
5. evaluate FPC's response immediately when received.

## Completion Criteria

OT-0001 remains open until at least one candidate is supported by sufficient evidence to select it as a development sensor, or the raw-sensor architecture must be reconsidered.

## Decision Output

Expected on success:

```text
ADR-0002 — Select OpenTouch Development Fingerprint Sensor
```

## Result

**Open — waiting on FPC vendor response.**
