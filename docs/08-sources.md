# Research Sources

Research snapshot updated: **2026-09-24**

## Apple biometric/security reference

### Apple — Biometric security

https://support.apple.com/guide/security/sec067eb0c9e/web

Supports:

- separation between biometric sensor and Secure Enclave;
- Secure Enclave performs template processing/storage/matching;
- raw raster scan exists temporarily in encrypted Secure Enclave memory and is discarded;
- enrolled biometric representation is encrypted and device-local;
- built-in Touch ID sensor↔Secure Enclave channel is encrypted and authenticated;
- per-sensor provisioned key material participates in establishing session protection.

### Apple — About Touch ID advanced security technology

https://support.apple.com/105095

Supports:

- Touch ID stores a mathematical representation rather than retained fingerprint images;
- fingerprint data is encrypted;
- it is protected by keys available only to the Secure Enclave;
- OS/apps do not get access;
- data is not sent to Apple or backed up to iCloud.

### Apple — The Secure Enclave

https://support.apple.com/guide/security/sec59b0b31ff/web

Supports:

- Secure Enclave as a hardware-isolated subsystem;
- secure boot/root-of-trust architecture;
- hardware cryptography;
- protected memory;
- secure nonvolatile/anti-replay mechanisms.

These sources are used as architectural reference only. OpenTouch must not claim equivalence.

## Linux baseline

### libfprint supported devices

https://fprint.freedesktop.org/supported-devices.html

### libfprint developer docs

https://fprint.freedesktop.org/libfprint-dev/

### Fedora libfprint

https://packages.fedoraproject.org/pkgs/libfprint/libfprint/

## Candidate sensor vendors

### Fingerprint Cards

https://www.fpc.com/products/fingerprints/

Published product material for access sensors has historically documented capacitive 508-dpi devices with SPI interfaces. Exact current availability, register documentation, SDK terms, and open-source compatibility must be verified directly before selection.

### Goodix capacitive fingerprint sensors

https://www.goodix.com/en/product/sensors/fingerprint_sensors/capacitive_fingerprint_sensors

Goodix lists compact capacitive fingerprint sensors, including SPI-connected products. Public product listings do not by themselves prove open raw-frame access or permission to implement an independent driver.

## Benchmark MOC device

### libfprint `3274:8012`

Use official upstream support/source plus physical testing.

This device remains a benchmark/reference, not the target OpenTouch architecture.

## Evidence policy

For every hardware claim prefer:

1. manufacturer datasheet;
2. manufacturer engineering response;
3. upstream source;
4. authorized distributor;
5. reproducible teardown/hardware report;
6. community report;
7. marketplace description.

A production decision must not rely solely on marketplace or forum descriptions.
