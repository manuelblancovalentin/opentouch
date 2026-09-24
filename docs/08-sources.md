# Research Sources

Research snapshot date: **2026-09-24**

This file records sources supporting the initial feasibility documentation. Where possible, prefer manufacturer, upstream, distribution, standards-body, and statutory sources over reseller/community claims.

## Upstream Linux

### libfprint supported devices

https://fprint.freedesktop.org/supported-devices.html

Supports these repository claims:

- `3274:8012` appears in the current development supported-device list.
- It is identified there as `MAFP MOC Fingerprint Sensor`.
- The page explicitly warns that development-list support does not imply inclusion in every stable release.

### libfprint developer documentation

https://fprint.freedesktop.org/libfprint-dev/getting-started.html

Supports:

- libfprint performs device discovery and provides enrollment/verification operations.

## Fedora

### Fedora libfprint package

https://packages.fedoraproject.org/pkgs/libfprint/libfprint/

Snapshot observation on 2026-09-24:

- Fedora package pages showed `1.94.100` across Fedora 43–45.

This does **not** alone establish whether a particular device-support patch is included. Driver inclusion must be separately verified.

## GNOME

### Fingerprint login

https://help.gnome.org/gnome-help/session-fingerprint.html

Supports:

- GNOME provides fingerprint enrollment/login UI for supported hardware;
- password login remains available.

## Linux-first competitor

### ThinkPenguin TPE-F4500

https://www.thinkpenguin.com/gnu-linux/optical-usb-fingerprint-reader-gnulinux-edition-tpe-f4500

https://thinkpenguin.com/gnu-linux/optical-usb-fingerprint-reader-gnulinux-edition-support-documentation-tpe-f4500

Supports:

- a purposefully GNU/Linux-oriented external fingerprint reader already exists;
- it is documented around the fprint ecosystem;
- it is an optical reader with a conventional desktop-reader product concept.

This is why OpenTouch must not claim novelty merely from being a Linux fingerprint reader.

## FIDO biometric comparison

### YubiKey C Bio

https://www.yubico.com/product/yubikey-c-bio/

Snapshot observation:

- USB-C biometric FIDO authenticator;
- listed around $98 on the 2026-09-24 research pass.

Used only as a market/security architecture comparison. It is not a drop-in fprint/PAM reader.

## USB

### USB-IF developer resources

https://www.usb.org/developers

### USB logo/license information

https://www.usb.org/logo-license

Used for:

- VID/PID and USB branding/compliance research;
- reminder that USB identity and logo rights must not be improvised.

Exact commercial fee schedules should be rechecked at the time of product launch.

## Illinois biometric law

### Illinois Biometric Information Privacy Act

https://www.ilga.gov/legislation/ilcs/fulltext.asp?DocName=074000140K10

Used to motivate local-only biometric architecture and the recommendation that OpenTouch itself avoid collecting fingerprint images/templates.

This repository does not provide legal advice.

---

# Evidence rules for future research

When adding a technical claim, tag it mentally according to evidence quality:

1. **Primary** — manufacturer/upstream/standards/statute.
2. **Strong secondary** — authorized distributor or reproducible teardown.
3. **Community** — issue/forum/report.
4. **Marketplace** — seller listing.

Production decisions should not rely solely on levels 3–4.

For hardware compatibility, require exact model/part number and VID:PID whenever possible.
