# ADR-0001 — Adopt Raw-Sensor / OpenTouch-Controller Architecture

- **Status:** Accepted
- **Date:** 2026-09-24
- **Related actions:** OT-0001, OT-0002, OT-0003, OT-0004
- **Supersedes:** none
- **Superseded by:** none

## Context

OpenTouch began as a possible productization effort around an existing Linux-supported fingerprint reader/module, particularly USB `3274:8012` Microarray MAFP.

That route offered several advantages:

- existing upstream `libfprint` support;
- fast path to a working Linux prototype;
- integrated match-on-chip behavior;
- low development complexity;
- small existing retail form factors.

However, an integrated MOC module leaves critical parts of the biometric and security architecture under vendor control.

Those include potentially:

- where templates are stored;
- how templates are protected;
- whether raw images can be extracted;
- firmware trust;
- enrollment semantics;
- deletion/reset semantics;
- host/device authentication;
- replay resistance;
- multi-user behavior;
- device lifecycle.

OpenTouch is intended to become more than a Linux enclosure around an opaque biometric module.

The project owner also explicitly does **not** intend to design the capacitive fingerprint sensing ASIC itself. The intended hardware boundary is therefore between the commercial sensing IC/module and the OpenTouch controller.

## Decision

OpenTouch will target the following architecture:

```text
finger
  ↓
commercial raw fingerprint sensing IC/module
  ↓
OpenTouch-designed controller / PCB
  ├── sensor acquisition
  ├── biometric preprocessing
  ├── template generation
  ├── biometric matching
  ├── secure template protection
  ├── key management
  ├── firmware trust
  └── authenticated host protocol
  ↓
USB-C
  ↓
upstream Linux integration
```

The sensing component may remain proprietary commercial silicon.

OpenTouch should own, to the extent technically and legally practical:

- controller electronics;
- firmware;
- host protocol;
- biometric-data architecture;
- template placement/protection;
- Linux integration;
- qualification;
- product design.

USB `3274:8012` may still be acquired and used as a reference/benchmark device, but it is not the intended final architecture.

## Alternatives Considered

### A. Productize `3274:8012` directly

Architecture:

```text
Microarray MOC USB module
        ↓
OpenTouch enclosure / carrier / productization
        ↓
libfprint / fprintd / PAM
```

#### Advantages

- shortest development path;
- existing Linux support;
- lower firmware/software effort;
- lower initial engineering cost;
- high probability of quickly reaching a useful prototype;
- potentially better near-term financial return.

#### Disadvantages

- opaque biometric/security core;
- weak control over template-storage semantics;
- limited control over firmware/security behavior;
- supplier dependency;
- limited technical differentiation;
- difficult to second-source without changing architecture;
- community contribution concentrated in productization rather than the biometric stack.

### B. Raw commercial sensor + OpenTouch controller

Architecture:

```text
commercial sensing IC
        ↓
OpenTouch controller
        ↓
OpenTouch protocol
        ↓
Linux
```

#### Advantages

- explicit trust boundary;
- control over biometric data placement;
- ability to choose host-side or device-side matching;
- ability to implement secure boot, signed updates, protected keys, and authenticated communication;
- reusable architecture across multiple sensor vendors/products;
- stronger open-source contribution;
- stronger long-term technical differentiation;
- potentially reusable in later OpenTouch and Legatum hardware.

#### Disadvantages

- materially greater engineering effort;
- sensor integration may depend on vendor documentation;
- biometric algorithm work becomes an OpenTouch responsibility;
- longer time to market;
- higher probability of technical dead ends;
- higher initial development cost.

### C. Design the fingerprint sensing ASIC

This would include the capacitive sensing array, analog front end, readout, conversion, calibration, and sensor silicon.

#### Reason not selected

This changes OpenTouch into a custom mixed-signal ASIC research program and introduces fabrication, packaging, analog characterization, sensing-surface/passivation, yield, and ESD problems that are unnecessary for the current product objective.

It remains theoretically possible as a separate future research project but is outside OpenTouch's current scope.

## Rationale

Option B was selected because it provides the strongest combination of:

- long-term security control;
- architectural transparency;
- open-source/community value;
- product differentiation;
- future platform reuse;
- potential integration into other systems.

The additional engineering burden is accepted because OpenTouch is intended to be a serious open hardware platform rather than only a small commercial wrapper around existing MOC hardware.

A raw-sensor architecture also permits OpenTouch to decide independently:

- whether templates reside on-device or on the Linux host;
- whether host-stored templates are encrypted using device-held keys;
- where matching occurs;
- whether raw images leave the controller;
- whether an authenticator is portable across machines;
- how templates are bound to users/hosts;
- how enrollment and reset are authorized.

## Consequences

### Positive

OpenTouch can potentially publish and audit:

- PCB/schematic;
- controller firmware;
- USB protocol;
- libfprint integration;
- biometric data flow;
- key hierarchy;
- enrollment/delete/reset semantics.

The architecture can potentially survive a future sensor substitution if a sensor abstraction is maintained.

The controller may later become a reusable hardware security component for other OpenTouch or Legatum use cases.

### Negative

The project now inherits several problems previously hidden inside the MOC module:

- sensor bring-up;
- raw-image acquisition;
- image-quality characterization;
- biometric preprocessing;
- template generation;
- matching;
- secure template storage;
- controller sizing;
- firmware-update security;
- protocol security.

This substantially increases development time and risk.

## Assumptions

This decision assumes at least one commercial fingerprint sensor exists that:

1. exposes raw or sufficiently low-level fingerprint data;
2. can be integrated without a prohibited proprietary host dependency;
3. is available in prototype quantities;
4. has adequate electrical/mechanical documentation;
5. permits publication of an open implementation or at least an open controller interface;
6. has a plausible production supply path.

This assumption is **not yet proven**.

OT-0001 exists to test it.

## Open Questions

This ADR deliberately does not decide:

- exact fingerprint sensor;
- exact controller/MCU;
- biometric algorithm;
- final template format;
- host-side vs device-side production matching;
- device-local vs host-encrypted template storage;
- portability vs host binding;
- sensor↔controller cryptographic protection;
- final host protocol;
- FIDO2 support.

Each should be decided separately after evidence exists.

## Revisit Triggers

Reopen or supersede this ADR if any of the following occurs:

- no viable raw sensor permits sufficiently open integration;
- all viable sensors require a proprietary SDK incompatible with the project's goals;
- raw-sensor MOQ/pricing is commercially unreasonable;
- embedded biometric processing requires hardware that makes the product economically impractical;
- sensor vendors prohibit publication of required interface code;
- security analysis shows that the custom controller provides insufficient advantage over a documented MOC device;
- a commercially available MOC module appears with sufficiently open/documented firmware, protocol, key storage, supply chain, and security properties to eliminate most advantages of the custom controller;
- development cost threatens the viability of the project before an end-to-end prototype exists.

## Evidence

The decision is based on the initial OpenTouch feasibility research and architectural comparison documented in:

- `docs/00-project-thesis.md`
- `docs/03-hardware-survey.md`
- `docs/04-security-model.md`
- `docs/09-biometric-data-architecture.md`

Relevant external architectural reference:

- Apple Platform Security documentation on Touch ID and Secure Enclave separation. This is used only as an architectural reference for separating sensing from protected biometric processing/storage; OpenTouch does not claim equivalence with Apple's security architecture.

The immediate validation work is tracked by:

- `OT-0001 — Raw sensor feasibility scan`.
