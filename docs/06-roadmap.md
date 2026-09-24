# Project Roadmap

## Phase 0A — Raw-sensor feasibility

### Goal

Prove that at least one commercial fingerprint sensor can support an open OpenTouch controller stack.

### M0A.1 — Sensor market scan

Investigate exact candidate parts from:

- Fingerprint Cards;
- Goodix;
- other credible sensor vendors.

For each candidate determine:

- raw-image/low-level access;
- SPI/register documentation;
- SDK requirements;
- NDA constraints;
- firmware/blob requirements;
- sample availability;
- MOQ;
- pricing;
- lifecycle.

### M0A.2 — Select development sensor

**Exit criterion:** one sensor is sufficiently open and sourceable to justify PCB/controller development.

### M0A.3 — Acquire benchmark `3274:8012`

Optional but recommended.

Purpose:

- user-experience baseline;
- latency comparison;
- physical-size benchmark;
- Linux integration reference.

It is not the target architecture.

---

## Phase 0B — Security/data architecture

### Goal

Define where biometric data lives before production firmware is designed.

Resolve:

- host vs device matching;
- template storage;
- root-key location;
- encrypted host blobs vs device NVM;
- portability vs host binding;
- enrollment authorization;
- reset/recovery;
- firmware trust.

Artifact:

- `docs/09-biometric-data-architecture.md`
- architecture decision record(s)

---

## Phase 1 — Sensor development platform

### Goal

Capture usable fingerprint images from the chosen raw sensor.

### Actions

- sensor breakout/dev board;
- controller development board;
- power;
- SPI;
- reset/interrupt;
- acquisition firmware;
- raw-frame capture;
- image-quality characterization.

### Exit criterion

Repeatable, stable images from multiple fingers under normal touch conditions.

---

## Phase 2 — Linux host-matching prototype

### Goal

Prove end-to-end Linux authentication before implementing embedded matching.

Architecture:

```text
sensor → OpenTouch controller → USB → libfprint → host matching
```

### Actions

- define development USB protocol;
- implement libfprint imaging driver;
- enrollment;
- verification;
- GNOME/PAM qualification.

### Exit criterion

OpenTouch works as a functional Linux fingerprint reader using host-side biometric processing.

---

## Phase 3 — OpenTouch controller PCB

### Goal

Replace development boards with the intended compact electronics.

### Actions

- select MCU/SoC;
- schematic;
- PCB;
- USB-C;
- ESD;
- sensor connector/integration;
- debug/programming;
- prototype enclosure.

### Exit criterion

Compact OpenTouch hardware reproduces Phase-2 behavior.

---

## Phase 4 — Device-side biometric engine

### Goal

Move sensitive biometric processing out of Linux.

### Actions

- evaluate matcher;
- embedded image preprocessing;
- template generation;
- template matching;
- performance optimization;
- false-reject characterization;
- secure template representation.

### Exit criterion

Normal authentication no longer requires raw biometric data or templates to leave the device.

---

## Phase 5 — Secure authenticator architecture

### Goal

Harden the device.

### Actions

- secure boot;
- signed updates;
- anti-rollback;
- root device key;
- template encryption;
- authenticated host protocol;
- replay resistance;
- enrollment authorization;
- secure reset;
- debug lockdown.

### Exit criterion

Documented security properties are enforced by the implementation rather than assumed.

---

## Phase 6 — Upstream/product qualification

### Actions

- upstream libfprint driver/protocol;
- fprintd/PAM testing;
- multi-distro qualification;
- suspend/resume;
- multi-user behavior;
- long-run testing;
- manufacturing tests.

---

## Phase 7 — Commercial product

### Actions

- DFM;
- sourcing;
- compliance;
- packaging;
- QA;
- warranty;
- retail pricing;
- launch.

---

## Phase 8 — Future integrations

Potential:

- keyboard;
- Legatum local hardware authentication;
- OEM module;
- embedded systems.

These are intentionally downstream of the standalone architecture.
