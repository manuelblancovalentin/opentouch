# Project Thesis

## 1. Project definition

OpenTouch is an open Linux-first fingerprint authenticator built from:

- a commercial raw fingerprint sensing IC/module;
- an OpenTouch-designed PCB and controller;
- OpenTouch firmware;
- an explicit biometric-data/security architecture;
- an open host protocol;
- upstream Linux integration through `libfprint` / `fprintd` / PAM.

The project does **not** attempt to fabricate the capacitive sensing ASIC itself.

## 2. Revised contribution

The previous architecture considered productizing an existing MOC module such as USB `3274:8012`.

That remains useful as a benchmark, but it is no longer the preferred product architecture because it leaves critical decisions inside an opaque vendor module:

- template storage;
- transport security;
- firmware trust;
- enrollment semantics;
- deletion semantics;
- device-side matching behavior.

OpenTouch instead aims to own the system **above the sensing silicon**.

## 3. Intended architectural boundary

OpenTouch purchases a commercial sensor that performs the physical fingerprint measurement and exposes the resulting image or equivalent low-level biometric data.

OpenTouch owns:

- sensor control;
- acquisition firmware;
- biometric preprocessing;
- template generation;
- matching architecture;
- template storage;
- key management;
- firmware trust;
- host communication protocol;
- USB integration;
- Linux driver/integration;
- qualification.

## 4. Product thesis

> **OpenTouch is an auditable Linux-first fingerprint authenticator whose electronics, firmware, protocol, biometric data model, and host integration are designed openly around a commercial fingerprint sensing IC.**

## 5. Why this is materially different

A polished wrapper around `3274:8012` could become a useful product quickly, but its core biometric/security architecture would remain somebody else's black box.

The raw-sensor architecture creates a reusable platform that could later support:

- standalone USB-C readers;
- keyboards;
- embedded Linux systems;
- workstations;
- access-control peripherals;
- future Legatum hardware;
- alternative sensing ICs behind a stable OpenTouch controller/protocol abstraction.

## 6. Non-goals

OpenTouch is not initially:

- a custom CMOS fingerprint sensing ASIC;
- a cloud biometric service;
- a replacement for PAM;
- a proprietary Linux daemon;
- a FIDO2 implementation;
- a promise of Apple-equivalent security;
- a new fingerprint-recognition research program unless existing algorithms are insufficient.

## 7. Design principles

1. **Sensor is replaceable.** Avoid coupling the whole architecture to one sensing vendor.
2. **Protocol is documented.** Host/device behavior should be inspectable.
3. **No biometric cloud.** Fingerprint data remains local.
4. **Security claims follow evidence.**
5. **Upstream first.** Avoid private libfprint/fprintd forks.
6. **Raw-image exposure is deliberate, not accidental.**
7. **Template ownership is explicit.**
8. **Password fallback remains available.**
9. **Hardware revision is fixed and disclosed.**
10. **The reference design must remain manufacturable.**

## 8. Success criteria

The project reaches architectural viability when:

- a raw commercial sensor is chosen;
- its acquisition interface is usable without a prohibited closed host dependency;
- the PCB/controller can acquire stable fingerprint data;
- an end-to-end Linux prototype can enroll and verify;
- template placement and key ownership are explicitly defined;
- secure firmware/update architecture is specified;
- the resulting design has a plausible BOM and sourcing path.

## 9. Benchmark role of `3274:8012`

USB `3274:8012` remains valuable as a reference for:

- physical size;
- latency;
- Linux UX;
- MOC behavior;
- enrollment flow;
- template capacity;
- practical user expectations.

It is no longer the presumed OpenTouch production module.
