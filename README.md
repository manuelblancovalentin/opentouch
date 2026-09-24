# OpenTouch

> A tiny match-on-chip USB-C fingerprint button for Linux, built around upstream `libfprint` / `fprintd` and PAM, without a proprietary host driver.

OpenTouch is an open-source hardware/productization project. It is **not** an attempt to invent fingerprint authentication for Linux. The Linux software stack already exists:

```text
fingerprint hardware
    ↓
libfprint
    ↓
fprintd
    ↓
pam_fprintd
    ↓
sudo / login / lock screen / other PAM consumers
```

The project asks a narrower question:

> Can that stack be packaged into a tiny, polished, reliable, Linux-first USB-C fingerprint button using stable, upstream-supported hardware?

## Current project thesis

The broad product category already exists. ThinkPenguin sells an explicitly GNU/Linux-oriented external fingerprint reader that works through the fprint stack. Therefore the defensible OpenTouch gap is **not** “a fingerprint reader for Linux.”

The current target is instead:

- tiny touch-button form factor;
- USB-C;
- preferably match-on-chip (MOC);
- upstream `libfprint` support;
- no proprietary Linux daemon or private driver fork;
- fixed, documented hardware identity;
- reproducible Linux qualification;
- consumer-quality enclosure and UX;
- sourceable enough to become a small-batch product.

The current leading hardware lead is USB VID:PID **`3274:8012`**, exposed upstream as the **MAFP MOC Fingerprint Sensor**.

## Documentation

- [`docs/00-project-thesis.md`](docs/00-project-thesis.md) — scope, gap, non-goals, success criteria
- [`docs/01-linux-stack.md`](docs/01-linux-stack.md) — Linux software baseline and integration model
- [`docs/02-competitive-landscape.md`](docs/02-competitive-landscape.md) — existing products and the surviving gap
- [`docs/03-hardware-survey.md`](docs/03-hardware-survey.md) — sensor/module candidates and qualification rules
- [`docs/04-security-model.md`](docs/04-security-model.md) — threat model and security claims policy
- [`docs/05-commercial-compliance.md`](docs/05-commercial-compliance.md) — open-source boundary, economics, compliance
- [`docs/06-roadmap.md`](docs/06-roadmap.md) — phases, milestones, and exit criteria
- [`docs/07-action-register.md`](docs/07-action-register.md) — ordered actionable work
- [`docs/08-sources.md`](docs/08-sources.md) — research sources and claim mapping

## Current status

**Phase 0 — Research / feasibility**

The immediate project gate is:

> Determine exactly what `3274:8012` is, whether a stable OEM module can be sourced, and whether it behaves acceptably with current upstream Linux software.

No custom PCB should be started before that gate is resolved.
