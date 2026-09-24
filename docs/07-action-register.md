# Action Register

Statuses: `TODO`, `IN PROGRESS`, `BLOCKED`, `DONE`, `DROPPED`

Priorities: `P0`, `P1`, `P2`, `P3`

---

## OT-0001 — Raw sensor feasibility scan

**Priority:** P0  
**Status:** TODO  
**Phase:** 0A

Evaluate exact raw commercial fingerprint sensors.

Initial families:

- Fingerprint Cards FPC1020 / FPC1024 / FPC1025;
- Goodix GF3988 / GF5288 / GF3258 / GF3626;
- additional candidates if superior.

For every candidate collect:

- exact P/N;
- public datasheet;
- image resolution;
- dimensions;
- SPI/electrical details;
- raw-frame accessibility;
- required firmware;
- required SDK;
- NDA restrictions;
- permission to publish open firmware/driver;
- distributor/sample path;
- MOQ;
- prototype cost;
- 100/1k pricing;
- lifecycle.

### Decision

Each candidate receives:

- `OPEN_DEV_CANDIDATE`
- `CLOSED_SDK_ONLY`
- `PROCUREMENT_RISK`
- `REJECTED`

---

## OT-0002 — Select OpenTouch development sensor

**Priority:** P0  
**Status:** BLOCKED by OT-0001

Selection requires:

- accessible raw/low-level sensor data;
- open implementation legally possible;
- prototype units obtainable;
- sane electrical integration;
- plausible production path.

---

## OT-0003 — Select controller-development platform

**Priority:** P0  
**Status:** BLOCKED by OT-0002

Do not select MCU solely by intuition.

Requirements depend on:

- image dimensions;
- buffering;
- preprocessing;
- matcher;
- secure-storage plan.

Evaluate:

- RAM;
- flash;
- USB;
- SPI throughput;
- crypto accelerators;
- secure boot;
- key storage;
- anti-rollback;
- debug lifecycle;
- price.

---

## OT-0004 — Decide biometric data architecture

**Priority:** P0  
**Status:** IN PROGRESS

Use `09-biometric-data-architecture.md`.

Resolve:

- development host matching;
- production device matching;
- template storage model;
- root-key location;
- encrypted host blob vs device NVM;
- host binding vs portability;
- enrollment authorization;
- reset/recovery.

---

## OT-0005 — Acquire `3274:8012` benchmark

**Priority:** P1  
**Status:** TODO

Purpose is benchmark/reference only.

Measure:

- physical design;
- Fedora behavior;
- latency;
- enrollment;
- match UX;
- suspend/resume;
- multi-user behavior.

Do not treat its architecture as OpenTouch's target.

---

## OT-0006 — Capture first raw fingerprint frame

**Priority:** P0  
**Status:** BLOCKED by OT-0002/0003

Deliverables:

- sensor bring-up;
- deterministic initialization;
- raw frame dump;
- metadata;
- acquisition timing;
- image visualization;
- repeated-frame test.

---

## OT-0007 — Build host-matching Linux prototype

**Priority:** P0  
**Status:** BLOCKED by OT-0006

Architecture:

```text
raw sensor → controller → USB → libfprint host image path
```

Goal:

Prove hardware and Linux integration before embedded matching.

---

## OT-0008 — Evaluate biometric algorithm path

**Priority:** P1  
**Status:** TODO after raw images exist

Compare:

- libfprint image/minutiae path;
- NBIS;
- SourceAFIS concepts/implementations;
- vendor algorithms if legally useful;
- custom embedded implementation only if necessary.

Criteria:

- accuracy;
- memory;
- compute;
- license;
- embedded suitability;
- template format;
- auditability.

---

## OT-0009 — Prototype secure template storage

**Priority:** P1  
**Status:** BLOCKED by OT-0008

Compare:

1. controller-local encrypted NVM;
2. encrypted Linux-hosted blobs bound to device key.

Test:

- key loss;
- rollback;
- deletion;
- reset;
- cross-device substitution;
- cross-user substitution.

---

## OT-0010 — Define production host protocol

**Priority:** P1  
**Status:** BLOCKED by security architecture

Protocol must eventually support at minimum:

- capability query;
- enrollment;
- verify;
- list credential identifiers;
- delete;
- reset;
- firmware/version state.

Security design must include:

- authenticated messages;
- freshness;
- versioning;
- error semantics.

---

## OT-0011 — Build compact OpenTouch PCB

**Priority:** P1  
**Status:** BLOCKED by sensor/controller validation

Only begin after development hardware proves:

- image quality;
- Linux path;
- power requirements;
- controller sizing.

---

# Immediate next action

**OT-0001 — Raw sensor feasibility scan.**

The project is now committed to the raw-sensor/custom-controller direction unless the market scan establishes that no practical sensor can be openly integrated.
