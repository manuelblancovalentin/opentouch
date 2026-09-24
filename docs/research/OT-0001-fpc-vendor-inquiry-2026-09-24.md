# OT-0001 Research Note — FPC Vendor Inquiry Sent

- **Date:** 2026-09-24
- **Related action:** OT-0001 — Raw Sensor Feasibility Scan
- **Related ADR:** ADR-0001 — Adopt Raw-Sensor / OpenTouch-Controller Architecture
- **Vendor:** Fingerprint Cards / Precise Biometrics
- **Status:** RESPONSE PENDING

## Purpose

Record the vendor inquiry sent to resolve the remaining FPC openness/documentation gate.

Public-source research established that FPC is currently the strongest raw-sensor candidate, but it did not establish whether current low-level programming documentation may be used to publish an independent open-source sensor driver.

## Inquiry context

The inquiry explained that OpenTouch intends to:

- use an FPC capacitive fingerprint sensor;
- connect it to an OpenTouch-owned microcontroller;
- implement independent firmware;
- implement independent biometric processing/template handling;
- implement Linux integration;
- avoid depending on FPC's proprietary biometric matcher unless deliberately chosen later.

The inquiry explicitly distinguished:

```text
FPC sensor interface
```

from:

```text
FPC licensed biometric algorithm
```

The project is not asking FPC to disclose or relicense its proprietary biometric algorithm.

## Questions sent

The vendor was asked to clarify:

1. Which current FPC10 or FPC15 sensor is recommended for a new compact embedded fingerprint reader?
2. Can the recommended sensor provide raw or grayscale fingerprint image frames directly to a customer MCU over SPI or another documented interface?
3. Is the FPC biometric algorithm license optional if OpenTouch implements its own image processing, template generation, and matching?
4. Does raw image acquisition require proprietary binary firmware, SDK, or runtime software?
5. Is the low-level register/interface programming documentation available to customers?
6. Is an NDA required for that documentation?
7. Would OpenTouch be permitted to publish an independently written open-source MCU driver implementing the documented sensor interface?
8. Can the SPI/register protocol be implemented and distributed in open-source firmware?
9. Which development kit exposes the raw sensor interface rather than an integrated MOC solution?
10. What are sample availability, lifecycle status, MOQ, and indicative prototype / 100 / 1,000-unit pricing?
11. Which liveness/anti-spoofing capabilities, if any, are implemented by the sensor hardware itself and which depend on FPC's licensed biometric software?

## Why the reply matters

Questions 5–8 are the architectural gate.

A positive answer would strongly support:

```text
OPEN_DEV_CANDIDATE
```

for a current FPC sensor and would justify preparing:

```text
ADR-0002 — Select OpenTouch Development Fingerprint Sensor
```

A negative answer such as:

```text
full operation requires confidential vendor code
and independent open publication is prohibited
```

would materially weaken FPC and require continuation of OT-0001 with alternative vendors.

## Current classification

Until a reply is received:

```text
FPC:
OPEN_DEV_CANDIDATE
DOCUMENTATION_UNRESOLVED
VENDOR_RESPONSE_PENDING
```

Do not treat the inquiry itself as evidence that publication permission exists.

## Purchasing implication

Do not yet purchase a generic product described as an "FPC1020A fingerprint module."

Current retail searches show many FPC1020A products that combine the FPC sensing element with an additional controller and expose UART/USB commands for:

- enrollment;
- feature extraction;
- matching;
- template storage.

Those products do not provide the raw-sensor development path OpenTouch is trying to validate.

A useful purchase should be one of:

1. a bare/current FPC sensing IC/module with documented low-level access;
2. an official or suitable evaluation board exposing the raw sensor interface;
3. a benchmark device purchased explicitly for comparative testing.

Wait for FPC's recommendation before purchasing category 1 or 2.
