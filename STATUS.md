# OpenTouch — Project Status

**Last updated:** 2026-09-24  
**Current phase:** Phase 0A — Raw-sensor feasibility  
**Current architecture status:** Raw commercial fingerprint sensor + OpenTouch-designed controller/PCB  
**Current decision record:** ADR-0001 accepted

---

## 1. Current objective

Identify a commercial fingerprint sensing IC/module that gives OpenTouch enough technical and legal control to build an open controller stack around it.

The selected sensor must support a practical path toward:

```text
finger
  ↓
raw commercial fingerprint sensor
  ↓
OpenTouch controller
  ├── acquisition
  ├── biometric processing
  ├── template generation
  ├── matching
  ├── secure template storage
  ├── secure boot / signed firmware
  └── authenticated host protocol
  ↓
USB-C
  ↓
libfprint / fprintd / PAM
  ↓
Linux
```

The project is **not** designing the capacitive sensing ASIC itself.

---

## 2. Current canonical decisions

| ADR | Decision | Status |
|---|---|---|
| [ADR-0001](docs/decisions/ADR-0001-raw-sensor-opentouch-controller.md) | Use a raw commercial fingerprint sensor with an OpenTouch-owned controller/firmware stack | Accepted |

The authoritative decision ledger is [`docs/decisions/README.md`](docs/decisions/README.md).

---

## 3. Current action

### OT-0001 — Raw sensor feasibility scan

**Priority:** P0  
**Status:** IN PROGRESS

Investigate candidate raw fingerprint sensors, initially including:

- Fingerprint Cards FPC1020 / FPC1024 / FPC1025;
- Goodix capacitive SPI sensors;
- any superior commercial sensor discovered during the scan.

For each candidate determine:

- exact part number;
- raw-image or sufficiently low-level access;
- electrical interface;
- public/private documentation requirements;
- proprietary SDK or firmware requirements;
- NDA restrictions;
- whether an open driver/firmware implementation is legally possible;
- engineering sample availability;
- distributor availability;
- MOQ;
- prototype pricing;
- 100 / 1k pricing where obtainable;
- lifecycle status.

### OT-0001 exit condition

At least one sensor is classified:

```text
OPEN_DEV_CANDIDATE
```

with enough evidence to justify development hardware.

If no candidate passes, revisit ADR-0001 rather than silently compromising the architecture.

---

## 4. Next expected decision

The next ADR should only be created when OT-0001 provides enough evidence.

Expected subject:

> **ADR-0002 — Select OpenTouch development fingerprint sensor**

Do **not** pre-create an accepted decision before the research is complete.

---

## 5. Deferred decisions

These are known future decisions, not yet records:

- controller / MCU / SoC selection;
- host-side vs device-side production matching;
- template storage architecture;
- device portability vs host binding;
- secure host/device protocol;
- biometric algorithm implementation;
- production sensor choice if different from development sensor.

They should become ADRs only when there is a real decision to record.

---

## 6. Benchmark hardware

USB `3274:8012` Microarray MAFP remains useful as a benchmark/reference device.

It may be acquired to characterize:

- physical UX;
- latency;
- Linux integration;
- enrollment behavior;
- match behavior;
- multi-user behavior;
- suspend/resume;
- dimensions.

It is **not** the target production architecture under ADR-0001.

---

## 7. Repository governance

Use three distinct artifacts:

### Research documentation

Contains knowledge, evidence, measurements, and technical analysis.

Examples:

- hardware survey;
- security model;
- Linux stack;
- biometric-data architecture.

### OT action records / action register

Contains work that must be performed.

Examples:

- investigate raw sensors;
- capture first image;
- build host-matching prototype.

### ADRs

Contain decisions and their rationale.

An ADR should state:

- context;
- decision;
- alternatives;
- rationale;
- consequences;
- assumptions;
- open questions;
- revisit triggers;
- evidence.

Do not use an ADR as a task list, and do not use an OT action as the permanent record of a decision.
