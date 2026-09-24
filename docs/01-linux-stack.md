# Linux Software Baseline

## 1. Standard architecture

OpenTouch should preserve the standard Linux path:

```text
fingerprint device
    ↓
libfprint
    ↓
fprintd
    ↓
pam_fprintd
    ↓
PAM consumers
```

Typical consumers include:

- graphical login;
- lock screen;
- `sudo`;
- polkit-backed privilege prompts;
- other PAM-aware applications.

`libfprint` handles communication with supported fingerprint devices and exposes enrollment/verification functionality. `fprintd` provides the system service and PAM integration layer.

## 2. What already exists

For supported hardware, Linux already has the basic infrastructure for:

- device discovery;
- enrollment;
- verification;
- GNOME fingerprint enrollment/login;
- PAM authentication;
- password fallback.

Therefore a Phase-1 OpenTouch prototype may require **no upstream code contribution at all**.

That is not a failure. It would establish that the product can be built around upstream infrastructure rather than a private software stack.

## 3. Support must be described in layers

Never use the single word “supported” without identifying the layer.

Use these states:

| State | Meaning |
|---|---|
| `upstream-dev` | Device appears in current libfprint development support |
| `released-upstream` | Driver is contained in a released libfprint version |
| `packaged-fedora` | Fedora's packaged version contains that support |
| `detected` | Device is enumerated/detected on the target machine |
| `functional` | Enrollment and verification work |
| `qualified` | OpenTouch's full behavioral test matrix passes |

The official libfprint device page explicitly warns that its supported-device list refers to the **development version** and that not every driver is necessarily present in stable releases.

As of 2026-09-24, Fedora's package pages report `libfprint 1.94.100` for Fedora 43–45. Exact inclusion of a given driver must still be verified rather than inferred from the version string.

## 4. Fedora / GNOME baseline

GNOME exposes fingerprint enrollment for supported scanners and allows fingerprint login after enrollment.

Fedora packages the normal fprint stack. PAM configuration must be handled cautiously because authentication-stack mistakes can lock a user out.

OpenTouch should avoid hand-editing PAM files where distro-supported configuration mechanisms exist. Any installation helper should be:

- explicit;
- reversible;
- distro-aware;
- idempotent;
- safe under password fallback.

## 5. Qualification matrix

The prototype must test each path separately.

### 5.1 Device/basic

- `lsusb` identifies exact VID:PID.
- device is detected by packaged `libfprint`;
- device is detected by current upstream `libfprint`;
- enrollment succeeds;
- verification succeeds;
- duplicate enrollment behavior is understood;
- stored-print listing/deletion behavior is understood.

### 5.2 Desktop

- GNOME Settings enrollment;
- GDM login;
- GNOME lock-screen unlock;
- logout/login cycle;
- reboot;
- cold boot with device attached;
- boot without device;
- attachment after login.

### 5.3 PAM consumers

- `sudo`;
- polkit prompts;
- terminal login if applicable;
- graphical privilege dialogs;
- password fallback;
- failed-fingerprint fallback;
- timeout behavior.

### 5.4 Multi-user

- enroll fingerprints for two local users;
- ensure one user's enrollment is not incorrectly usable for another;
- test user switching;
- test administrative enrollment permissions;
- inspect where per-user metadata and device-side templates live.

### 5.5 Lifecycle

- unplug/replug;
- repeated hotplug;
- suspend/resume;
- hibernate if enabled;
- service restart;
- crash/recovery behavior;
- device reset;
- long idle periods.

## 6. Reliability measurements

A product needs more than “it worked once.”

Record at minimum:

- enrollment attempts required;
- verification latency;
- failed verification rate in ordinary use;
- recovery after failed touch;
- orientation sensitivity;
- dry-finger behavior;
- damp-finger behavior;
- repeated scans;
- first scan after resume;
- first scan after cold boot.

No FAR/FRR security claim should be made from casual testing. Those terms require controlled biometric evaluation.

## 7. Software design rule

Do **not** introduce `opentouchd` unless a concrete missing function requires it.

Preferred architecture:

```text
OpenTouch hardware
    ↓
upstream libfprint
    ↓
upstream fprintd
    ↓
distribution PAM / desktop integration
```

The absence of a proprietary OpenTouch host daemon is intended to be a product feature.

## Sources

- libfprint supported devices: https://fprint.freedesktop.org/supported-devices.html
- libfprint getting started: https://fprint.freedesktop.org/libfprint-dev/getting-started.html
- Fedora libfprint package: https://packages.fedoraproject.org/pkgs/libfprint/libfprint/
- GNOME fingerprint login: https://help.gnome.org/gnome-help/session-fingerprint.html
