# OpenTouch

> An open Linux-first fingerprint authenticator built from a raw commercial fingerprint sensor, an OpenTouch-designed controller/PCB, and an upstream Linux integration path.

OpenTouch is now committed to a **raw-sensor architecture** rather than productizing an opaque match-on-chip USB module as the final design.

The intended stack is:

```text
finger
  ↓
commercial capacitive fingerprint sensor
  ↓  SPI / documented sensor interface
OpenTouch controller
  ├── acquisition
  ├── image conditioning
  ├── feature/template generation
  ├── matching
  ├── secure template storage
  ├── secure boot / signed firmware
  └── authenticated USB protocol
  ↓
USB-C
  ↓
libfprint / fprintd / PAM
  ↓
Linux
```

The `3274:8012` Microarray device remains useful as a **benchmark/reference device**, not the target architecture.

## Why this direction

The raw-sensor approach allows OpenTouch to define:

- where fingerprint templates are stored;
- how they are encrypted;
- whether raw images ever leave the device;
- how enrollment is authorized;
- how host↔device communication is authenticated;
- firmware trust and update policy;
- anti-rollback behavior;
- multi-user semantics;
- template deletion/reset semantics;
- the public USB protocol;
- the upstream Linux integration.

This makes OpenTouch an open biometric peripheral architecture rather than a Linux wrapper around another vendor's authenticator.

## Documentation

- [`docs/00-project-thesis.md`](docs/00-project-thesis.md) — revised project thesis and scope
- [`docs/01-linux-stack.md`](docs/01-linux-stack.md) — Linux software baseline
- [`docs/02-competitive-landscape.md`](docs/02-competitive-landscape.md) — product landscape
- [`docs/03-hardware-survey.md`](docs/03-hardware-survey.md) — raw-sensor selection strategy
- [`docs/04-security-model.md`](docs/04-security-model.md) — threat model
- [`docs/05-commercial-compliance.md`](docs/05-commercial-compliance.md) — commercial/open-source boundary
- [`docs/06-roadmap.md`](docs/06-roadmap.md) — revised roadmap
- [`docs/07-action-register.md`](docs/07-action-register.md) — current engineering queue
- [`docs/08-sources.md`](docs/08-sources.md) — research/source ledger
- [`docs/09-biometric-data-architecture.md`](docs/09-biometric-data-architecture.md) — template placement, secure storage, and Apple comparison

## Current project gate

**Phase 0A — raw-sensor feasibility**

Before committing PCB design effort, identify at least one commercial fingerprint sensor that:

1. provides raw or sufficiently low-level fingerprint access;
2. is documented enough to integrate without a proprietary host daemon;
3. is legally usable with an open firmware/driver stack;
4. is sourceable in prototype and production quantities;
5. has acceptable lifecycle and mechanical characteristics.

Primary candidates currently include Fingerprint Cards access sensors and Goodix capacitive SPI sensors.

The next architecture decision is **not** "Microarray or custom PCB." The project has already chosen the custom-controller path. The next decision is:

> Which raw commercial sensor gives OpenTouch enough technical and legal control to build the rest of the stack openly?
