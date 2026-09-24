# Security Model

## 1. Security objective

OpenTouch should control the trust boundary rather than inherit it from an opaque fingerprint module.

A raw-sensor architecture allows separate decisions about:

- capture;
- biometric processing;
- template creation;
- template storage;
- matching;
- key storage;
- firmware trust;
- host/device authentication.

## 2. Reference architecture

```text
fingerprint sensor
       ↓
OpenTouch trusted controller
       ├── image processing
       ├── template creation
       ├── template matching
       ├── key management
       └── secure state
       ↓
authenticated USB protocol
       ↓
Linux host
```

The security goal is **not** that the sensor itself be trusted with the whole authentication decision.

## 3. Template-placement options

### A. Host-side templates

```text
sensor → controller → Linux → encrypted template on host
```

Advantages:

- simplest development;
- easiest integration with existing libfprint image/minutiae machinery;
- powerful host CPU;
- easy debugging.

Disadvantages:

- compromised root/kernel may gain access to biometric material;
- templates must be protected by host storage/key architecture;
- difficult to claim isolation from the OS.

Recommended use: early development only.

### B. Device-side templates

```text
sensor → OpenTouch controller → encrypted local template store
```

Advantages:

- template can remain outside Linux;
- authentication can work through a yes/no operation;
- consistent across hosts if desired;
- OS compromise need not expose the stored template.

Disadvantages:

- controller must implement secure storage;
- stolen-device attack becomes important;
- multi-user mapping and lifecycle become device concerns.

This is the leading production direction.

### C. Host storage encrypted to device-held key

```text
encrypted template blob on Linux disk
         ↓
usable only by OpenTouch controller/device key
```

Advantages:

- device does not need large nonvolatile storage;
- templates remain cryptographically unusable without the authenticator;
- easier backup/versioning mechanics if deliberately supported.

Disadvantages:

- rollback/replay of old encrypted blobs must be handled;
- host can delete/copy blobs;
- secure anti-replay counters or state may still be required on device.

This architecture deserves serious evaluation.

## 4. Apple as a reference, not a target to copy blindly

Apple's documented architecture separates the biometric sensor from the Secure Enclave.

The sensor captures fingerprint data and sends it through a protected channel. The Secure Enclave performs biometric processing, protects the template, and performs matching.

Apple states that fingerprint templates are encrypted, stored on device storage, and protected by keys available only to the Secure Enclave. The OS and applications cannot access the biometric template.

For built-in Touch ID, Apple documents an encrypted and authenticated connection between the sensor and Secure Enclave using per-device provisioned key material and session-key establishment.

This suggests an important OpenTouch design pattern:

> The raw fingerprint sensor need not itself be the secure storage device. A separate trusted controller can be the biometric-security boundary.

OpenTouch should not claim equivalence with Apple's implementation. Apple's architecture includes a hardware root of trust, Secure Enclave isolation, secure boot, anti-replay mechanisms, factory sensor pairing, protected key storage, and extensive platform integration.

## 5. Potential OpenTouch trust boundary

A plausible long-term design is:

```text
RAW SENSOR
  captures image
     ↓
OPEN TOUCH SECURE CONTROLLER
  - authenticates sensor if possible
  - receives image
  - builds template
  - performs match
  - owns device key
  - controls template encryption
  - enforces enrollment authorization
  - secure boots signed firmware
     ↓
LINUX
  asks for enroll / verify / delete / status
```

Linux would not receive a raw image or template in normal production operation.

## 6. Development staging

### Dev stage

Host-side matching is acceptable to prove:

- sensor acquisition;
- image quality;
- PCB;
- USB path;
- Linux integration.

### Production stage

Move:

- feature extraction;
- template generation;
- matching;
- template protection;

into the OpenTouch controller if performance and algorithm quality permit.

This keeps early engineering tractable without freezing the final security model.

## 7. Required properties for production

Target properties:

- secure boot;
- signed updates;
- anti-rollback;
- hardware-backed device identity;
- protected key material;
- authenticated host/device protocol;
- freshness/nonces on verification operations;
- explicit enrollment authorization;
- deterministic template deletion;
- secure factory reset;
- debug interface lifecycle control.

Desired but separately validated:

- sensor/controller authenticated channel;
- PAD/liveness;
- physical tamper resistance.

## 8. Threats

OpenTouch must analyze:

- malicious Linux root;
- malicious USB host;
- malicious firmware;
- stolen authenticator;
- copied encrypted template storage;
- rollback of template/state;
- replayed verification messages;
- unauthorized enrollment;
- spoof fingerprint;
- compromised sensor;
- supply-chain substitution.

## 9. Security claims policy

Never infer:

- liveness;
- non-exportability;
- anti-replay;
- secure element;
- encrypted sensor link;
- firmware authenticity;

merely from the presence of a capacitive sensor or local matching.

Every claim requires architectural or implementation evidence.
