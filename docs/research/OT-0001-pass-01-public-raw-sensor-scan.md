# OT-0001 Research Note — Public Raw-Sensor Scan, Pass 1

- **Date:** 2026-09-24
- **Related action:** OT-0001 — Raw Sensor Feasibility Scan
- **Related decision:** ADR-0001 — Adopt Raw-Sensor / OpenTouch-Controller Architecture
- **Research status:** Public-source pass complete; vendor confirmation still required

## Purpose

Record the first public-source evidence pass for commercially available raw fingerprint sensors suitable for the OpenTouch architecture.

This note is evidence for OT-0001. It is **not** itself a decision record.

## Executive finding

Fingerprint Cards (FPC) is currently the strongest candidate family for an open OpenTouch controller.

Goodix remains technically interesting but is materially weaker under the project's openness requirement because public evidence indicates that detailed fingerprint integration documentation has historically been NDA-restricted.

Current ordering:

1. **FPC1020 / FPC10-family** — strongest development lead
2. **FPC1025 / current equivalent** — strong product-form candidate, documentation still to verify
3. **FPC1523 / current FPC15-family equivalent** — compact product candidate, lower pixel count
4. Goodix SPI sensors — technically plausible but documentation-access risk

No development sensor has yet been selected.

## 1. Fingerprint Cards

### 1.1 Architecture fit

FPC's current fingerprint-sensor offering explicitly supports integration onto a customer's own MCU and memory architecture.

This is compatible with the OpenTouch target:

```text
FPC sensing IC/module
        ↓
OpenTouch MCU/controller
        ↓
OpenTouch firmware/security architecture
        ↓
USB-C
        ↓
Linux
```

FPC also separates the physical sensor product from its optional/licensed biometric software stack.

This is important because OpenTouch does not require the vendor's matcher if raw sensor data can be independently acquired.

### 1.2 Historical raw-image evidence

Historical GPL-licensed FPC1020 Linux driver code carrying Fingerprint Cards AB copyright contains direct register-level sensor control.

The implementation performs operations including:

- SPI communication;
- register writes;
- sensor configuration;
- explicit image-capture commands;
- image retrieval;
- image cropping;
- ADC gain/shift configuration.

This is strong evidence that the FPC1020 generation can expose fingerprint image data directly rather than only returning a match result.

It does **not** prove that every current FPC sensor is publicly programmable under equivalent terms.

### 1.3 Public sensor specifications

Published FPC Access Sensor Series material has described the following devices:

| Part | Pixel array | Resolution | Approx. form factor | Supply | Published capture time |
|---|---:|---:|---|---|---:|
| FPC1020 | 192 × 192 | 508 dpi | 14 × 14 mm square | 1.8 / 3.3 V | <60 ms |
| FPC1024 | 192 × 192 | 508 dpi | Ø16 mm | 1.8 / 3.3 V | <60 ms |
| FPC1025 | 160 × 160 | 508 dpi | Ø14 mm | 1.8 / 3.3 V | <50 ms |
| FPC1523 | 96 × 96 | 508 dpi | Ø11 mm | 1.8 V | <25 ms |

The published family documentation describes SPI-connected capacitive fingerprint sensors producing grayscale fingerprint-image data.

### 1.4 Development ordering

#### FPC1020

Current role:

```text
LEADING DEVELOPMENT REFERENCE
```

Reasons:

- strongest evidence of direct raw image acquisition;
- known register-level implementation exists historically;
- 192 × 192 image gives a useful development baseline;
- larger physical package is acceptable for a development board.

Primary unresolved issue:

- current availability/recommended replacement and documentation terms.

#### FPC1025

Current role:

```text
LEADING PRODUCT-FORM CANDIDATE
```

Reasons:

- smaller round form factor;
- 160 × 160 image;
- same general sensor family concept.

Unresolved:

- exact current-programming documentation;
- raw-frame access terms;
- whether it remains recommended for new design.

#### FPC1523

Current role:

```text
COMPACT PRODUCT CANDIDATE
```

Advantages:

- approximately 11 mm diameter;
- very attractive for a small finished OpenTouch device.

Concern:

- 96 × 96 pixels provides a substantially smaller fingerprint area.

This should not be selected solely for industrial design before image/matching performance is evaluated.

### 1.5 Current FPC commercial structure

FPC currently organizes its raw-sensor business into families such as:

- FPC10;
- FPC13;
- FPC15.

The current vendor positioning still includes raw fingerprint sensor modules intended for customer integration.

Authorized distribution includes major electronics channels such as Digi-Key, Mouser, and Future Electronics.

This materially reduces procurement risk relative to opaque commodity modules.

### 1.6 FPC unresolved gate

The public evidence does **not** yet establish that a new OpenTouch design may obtain the complete current register/programming documentation under terms compatible with publishing an open implementation.

A historical sensor-integration document found through distribution channels is marked confidential/proprietary.

Therefore the key distinction is:

```text
technical feasibility: strongly supported
open publication/legal feasibility: unresolved
```

### 1.7 Current FPC classification

```text
OPEN_DEV_CANDIDATE
DOCUMENTATION_UNRESOLVED
```

Confidence: medium.

The `OPEN_DEV_CANDIDATE` classification is provisional and must be confirmed through vendor documentation/terms.

---

## 2. Goodix

### 2.1 Hardware attractiveness

Goodix currently lists several active capacitive SPI-connected fingerprint sensors with physically attractive packages.

Public candidates include:

- GF3626
- GF3988
- GF5288
- GF3258

Representative published sizes are in the approximately 10–15 mm range, with some very thin packages.

The GF3988-class form factor is especially attractive for a compact OpenTouch product.

### 2.2 Documentation problem

Public Goodix developer correspondence has stated that detailed fingerprint documentation is available only under NDA and is not publicly released.

That creates a direct risk for OpenTouch because the project intends to publish the controller/firmware integration where legally possible.

Open-source reverse-engineering projects show that some Goodix SPI sensors can be operated at a low level, but reverse engineering is not equivalent to a clean vendor-supported open integration path.

### 2.3 Current Goodix classification

For the current public evidence:

```text
DOCUMENTATION_BLOCKED
CLOSED_VENDOR_STACK_RISK
```

Not `REJECTED`.

Goodix may be reconsidered if the vendor explicitly permits an open independent sensor driver or provides documentation under compatible terms.

---

## 3. Integrated FPC products as economic reference

FPC also sells integrated biometric modules/SiPs such as AllKey-family products combining:

- fingerprint sensing;
- processing;
- storage;
- matching.

These are not the selected OpenTouch architecture.

They are useful as an economic sanity reference: an open sensor + MCU + storage architecture should not be allowed to become arbitrarily expensive merely because OpenTouch owns the stack.

The integrated-product pricing should later be used when evaluating whether OpenTouch's controller architecture creates enough value to justify its BOM.

---

## 4. Current candidate table

| Candidate | Low-level/raw evidence | Open implementation status | Procurement outlook | Classification |
|---|---|---|---|---|
| FPC1020 / FPC10 lineage | Strong | Historical open implementation; current terms unresolved | Relatively strong vendor/distributor ecosystem | `OPEN_DEV_CANDIDATE`, `DOCUMENTATION_UNRESOLVED` |
| FPC1025 / current equivalent | Strong family-level indication; exact current docs unresolved | Unresolved | Relatively strong | `OPEN_DEV_CANDIDATE`, `DOCUMENTATION_UNRESOLVED` |
| FPC1523 / current equivalent | Published image-sensor architecture; exact implementation terms unresolved | Unresolved | Relatively strong | `OPEN_DEV_CANDIDATE`, `DOCUMENTATION_UNRESOLVED` |
| Goodix GF3988 | SPI publicly stated; raw protocol not publicly established | NDA/documentation risk | Unknown | `DOCUMENTATION_BLOCKED` |
| Goodix GF5288 | SPI publicly stated | NDA/documentation risk | Unknown | `DOCUMENTATION_BLOCKED` |
| Goodix GF3258 | Reverse-engineering evidence exists | Vendor openness unresolved | Unknown | `DOCUMENTATION_BLOCKED` |
| Goodix GF3626 | Low-level research exists | Vendor secure/closed stack risk | Unknown | `DOCUMENTATION_BLOCKED` |

## 5. Evidence still required before ADR-0002

The following questions must be resolved for FPC before selecting a development sensor.

1. Which currently recommended part should be used for a new design?
2. Can the current sensor provide grayscale/raw fingerprint frames directly to a customer MCU?
3. Is full register/interface documentation available?
4. Is FPC's biometric matcher optional?
5. Is any binary firmware/SDK required merely to capture images?
6. Is an NDA required for programming documentation?
7. If an NDA is required, may OpenTouch still publish its independently written MCU driver?
8. May the documented SPI/register interface be implemented in open-source firmware?
9. Which evaluation/development board exposes the raw sensor rather than only an integrated matcher?
10. What are sample availability, MOQ, lifecycle, and indicative 100/1k pricing?

Questions 6–8 are the critical openness gate.

## 6. Next research step

Prepare a focused engineering inquiry to Fingerprint Cards / Precise Biometrics covering the ten questions above.

In parallel, perform one additional vendor scan specifically for fingerprint sensor suppliers that advertise:

- raw image output;
- SPI or similarly low-level interface;
- public programming documentation;
- independent MCU integration.

The purpose of the parallel scan is to avoid treating FPC as the only possible supplier before ADR-0002.

## 7. Sources

Primary/current:

- Fingerprint Cards sensor products:
  https://www.fpc.com/products/fingerprints/fingerprint-sensors/
- Fingerprint Cards distribution:
  https://www.fpc.com/products/distributors/
- Goodix capacitive fingerprint sensors:
  https://www.goodix.com/en/product/sensors/fingerprint_sensors/capacitive_fingerprint_sensors
- Goodix developer response regarding fingerprint documentation:
  https://developers.goodix.com/en/bbs/detail/861d7e6fe1d44cd4876ad5aade350b12

Implementation/history:

- Historical FPC1020 GPL driver:
  https://github.com/SanniZ/fpc1020-driver
- Goodix SPI Linux reverse-engineering:
  https://github.com/wrobelda/goodix-fp-spi-linux

Product-family data:

- FPC Access Sensor Series product sheet:
  https://www.fingerprints.com/uploads/2022/12/fingerprints_productsheet_accesssensorseries_fpc1020_fpc10124_fpc1025_fpc1523-1.pdf

## 8. Research conclusion

OT-0001 remains open.

Current best hypothesis:

> FPC provides a credible path to an OpenTouch raw-sensor architecture, but current documentation/publication rights must be confirmed before a development sensor is selected.

No ADR-0002 should be accepted yet.
