# Project Thesis

## 1. Problem statement

Linux already has a standard fingerprint-authentication path through `libfprint`, `fprintd`, and PAM. Configuring a supported reader on Fedora is useful, but configuration alone is not a meaningful OpenTouch contribution.

The product problem is instead the absence — or scarcity — of a peripheral combining all of the following:

- tiny touch-button form factor;
- USB-C;
- deliberate Linux support;
- upstream `libfprint` / `fprintd`;
- ordinary PAM integration;
- stable and disclosed hardware identity;
- preferably match-on-chip operation;
- no proprietary host daemon;
- no required cloud account;
- reproducible hardware and Linux qualification;
- realistic small-batch manufacture.

## 2. Revised product thesis

The initial thesis, “a Linux fingerprint reader,” is too broad.

An explicitly Linux-first external fingerprint reader already exists: ThinkPenguin's TPE-F4500 is sold for GNU/Linux and documented around the fprint stack.

Therefore the surviving product thesis is:

> **OpenTouch is a tiny match-on-chip USB-C fingerprint button for Linux, using the upstream fprint stack without a proprietary host driver.**

The product advantage must come from **form factor, known-good hardware, reproducibility, qualification, integration quality, and support**, not merely from the existence of Linux fingerprint authentication.

## 3. What OpenTouch is not

OpenTouch is not initially:

- a new biometric matching algorithm;
- a replacement for `libfprint`;
- a replacement for `fprintd`;
- a custom PAM framework;
- a proprietary Linux daemon;
- a cloud biometric service;
- a FIDO2 key;
- a claim of Apple Touch ID-equivalent security;
- a keyboard project.

A keyboard may become a later integration target only after the standalone device is validated.

## 4. Contribution layers

A meaningful contribution can exist at several layers.

### 4.1 Hardware/productization

Potential contributions:

- tiny enclosure;
- USB-C integration;
- carrier/custom PCB;
- qualified fingerprint module;
- open CAD;
- open schematic/PCB/BOM where appropriate;
- manufacturing test fixtures;
- stable physical design.

### 4.2 Linux integration

Potential contributions:

- distro qualification;
- setup tooling;
- `udev` or system integration if actually needed;
- suspend/resume qualification;
- login / lock / `sudo` / polkit behavior;
- multi-user qualification;
- diagnostics;
- packaging/documentation.

### 4.3 Upstream software

Only if required:

- new VID:PID support;
- `libfprint` driver fixes;
- `fprintd` fixes;
- PAM/documentation fixes;
- tests.

Private forks are explicitly undesirable.

### 4.4 Security architecture

Potential contribution:

- choose MOC where justified;
- characterize where templates reside;
- characterize raw-image exposure;
- characterize transport security;
- document replay/spoofing limitations;
- provide a precise threat model;
- refuse unsupported marketing claims.

## 5. Product value hypothesis

The commercial value need not be proprietary source code.

Possible defensible value:

- fixed hardware revision;
- controlled sourcing;
- tested VID:PID;
- known upstream compatibility;
- mechanical quality;
- automated qualification;
- distro support matrix;
- warranty;
- documentation;
- fulfillment;
- brand trust.

The working hypothesis is that Linux users may pay a premium to avoid the normal fingerprint-reader failure modes:

- undisclosed chipset;
- silent hardware revisions;
- vendor Windows-only software;
- unsupported hardware IDs;
- abandoned proprietary drivers;
- inconsistent PAM/desktop behavior.

## 6. Success criteria

OpenTouch should not be called a viable product until all of the following are true:

1. A specific sensor/module is identifiable by exact model and hardware ID.
2. The module is legally and repeatably sourceable.
3. Upstream Linux support is verified for the exact hardware.
4. Stock-distribution support status is known separately from upstream support.
5. Enrollment and verification are reliable.
6. Login, lock screen, `sudo`, polkit, multi-user behavior, hotplug, and suspend/resume are tested.
7. The security architecture is documented without unsupported claims.
8. A custom physical design can be built without a proprietary host stack.
9. Landed COGS supports a plausible enthusiast retail price.
10. Compliance and vendor-ID issues are understood before commercial sale.

## 7. Kill criteria

The current architecture should be abandoned or changed if:

- the selected module cannot be sourced under sane terms;
- the module's identity or firmware changes unpredictably;
- Linux operation requires a proprietary host driver;
- reliability is materially worse than password fallback expectations;
- security behavior cannot be characterized enough to market honestly;
- a stable product requires a private `libfprint` fork;
- landed cost forces a retail price with no plausible market;
- support burden overwhelms the value of the product.

## 8. Current central question

The next project question is deliberately narrow:

> **What exactly is USB `3274:8012`, who makes the underlying module, how can it be sourced, and how does it behave on current Fedora/upstream libfprint?**
