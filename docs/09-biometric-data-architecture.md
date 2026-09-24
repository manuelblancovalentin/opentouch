# Biometric Data Architecture

## 1. Purpose

The raw-sensor decision makes biometric-data placement an explicit OpenTouch design question.

OpenTouch must decide:

- where raw fingerprint images exist;
- where templates are generated;
- where templates are stored;
- which component owns the encryption keys;
- where matching occurs;
- what Linux is allowed to see;
- how multi-user identity maps to templates;
- how deletion and reset work.

## 2. Data types

Distinguish these rigorously:

### Raw image

A captured grayscale fingerprint image or equivalent sensor frame.

This is the most reconstruction-sensitive biometric artifact.

### Feature representation

Intermediate ridge/minutiae/features extracted from the raw image.

May still contain substantial biometric information.

### Template

The enrolled representation used for later matching.

A template is not harmless merely because it is not a photograph.

### Match result

A bounded authentication result such as:

```text
MATCH / NO_MATCH
```

This is the least biometric-rich interface to expose to the host.

## 3. Architectural options

### Option H — Host-centric

```text
sensor
 ↓
OpenTouch bridge
 ↓
Linux
 ├── preprocess
 ├── template
 ├── store
 └── match
```

Best for development.

Weakest isolation from a compromised host.

### Option D — Device-centric

```text
sensor
 ↓
OpenTouch controller
 ├── preprocess
 ├── template
 ├── encrypted local store
 └── match
 ↓
Linux receives result
```

Strong candidate for production.

### Option E — Encrypted external storage

```text
sensor
 ↓
OpenTouch controller
 ├── template
 ├── device-held key
 └── crypto
 ↓
encrypted template blob stored by Linux
```

The Linux filesystem stores ciphertext, while only the authenticator can use the template.

This can reduce secure-NVM requirements on the controller but introduces rollback/state-management concerns.

## 4. Apple reference architecture

Apple's public security documentation provides a useful reference model.

Apple states that:

- Touch ID's sensor captures fingerprint data;
- biometric processing and matching occur under the Secure Enclave security boundary;
- stored fingerprint data is a mathematical representation rather than a retained fingerprint image;
- the template is encrypted;
- the key protecting it is available only to the Secure Enclave;
- the OS and applications cannot access the biometric data;
- biometric templates are not sent to Apple or included in backups.

Apple further documents that built-in Touch ID sensors communicate with the Secure Enclave over an encrypted and authenticated channel using sensor-specific provisioned key material.

This is important conceptually:

> Secure biometric storage does not require the sensing IC itself to be the long-term template store.

A separate trusted controller/security subsystem can own the biometric state.

## 5. What OpenTouch can borrow conceptually

Not Apple's proprietary implementation, but these design principles:

1. **separate sensing from trust;**
2. **do not expose templates to ordinary host software;**
3. **bind template decryption/use to a hardware-held key;**
4. **authenticate sensitive communications;**
5. **use secure boot for biometric firmware;**
6. **treat enrollment as a privileged security operation;**
7. **make deletion/reset cryptographically meaningful;**
8. **keep biometric data out of cloud services.**

## 6. OpenTouch design candidates

### Candidate 1 — Fully device resident

Store encrypted templates in controller-attached flash/NVM.

Possible design:

```text
device root key
    ↓ HKDF
template-encryption key
    ↓
AEAD(template, metadata, version, user-slot)
    ↓
device flash
```

Need:

- monotonic/anti-rollback state;
- flash wear analysis;
- secure erase semantics;
- protected root key.

### Candidate 2 — Linux-hosted encrypted blobs

Store ciphertext in a root-owned Linux path but bind encryption to the physical OpenTouch device.

Example concept:

```text
device root key
 + user/slot ID
 + template version
        ↓
KDF
        ↓
AEAD encrypted template
        ↓
Linux filesystem
```

Linux may copy or delete the blob but cannot decrypt or create a valid replacement without the device key.

Need protection against:

- rollback to older template blobs;
- cross-user substitution;
- cross-device substitution;
- stale enrollment resurrection.

### Candidate 3 — Linux hardware security backend

A later Legatum-oriented system could potentially place OpenTouch keys/template metadata under a host TPM or another hardware-backed local security service.

This should remain a separate future architecture because OpenTouch must work as a generic Linux peripheral without requiring Legatum.

## 7. Multi-user model

Potential mapping:

```text
Linux account
   ↓
opaque OpenTouch credential ID
   ↓
device template slot / encrypted template object
```

The host should not need to know biometric contents.

Questions:

- Should one device support fingerprints for multiple Linux users?
- Should templates travel with the device or be host-bound?
- Can the same finger intentionally enroll for multiple accounts?
- How are stale users removed?
- What happens if the device is moved to another machine?

These must be explicit policy decisions.

## 8. Portability vs security

There is a real product choice.

### Portable authenticator

Templates live on/with OpenTouch.

Move the device to another compatible host and the biometric identity travels.

Advantages:

- convenient;
- potentially useful for Legatum or multiple workstations.

Risks:

- stolen reader contains the authentication state;
- host/account binding becomes complex.

### Host-bound biometric peripheral

Templates are cryptographically bound to both the OpenTouch device and a specific Linux host/account.

Advantages:

- stolen device alone is less useful;
- better local-system authentication semantics.

Disadvantages:

- re-enrollment needed on another computer;
- less portable.

OpenTouch should not choose this accidentally.

## 9. Legatum relevance

The architecture should remain generic, but the raw-sensor path creates a useful future possibility for Legatum:

- a local hardware trust anchor;
- biometric authorization for privileged local operations;
- protected local secrets;
- user-presence confirmation;
- possibly device-bound encrypted credentials.

These are future integrations, not requirements for OpenTouch v1.

## 10. Immediate research questions

Before finalizing the biometric-data architecture:

- select controller candidates with real secure-key storage;
- determine image RAM/compute requirements;
- determine matcher memory/CPU requirements;
- evaluate local-template vs encrypted-host-blob storage;
- decide device portability policy;
- define enrollment authorization;
- define reset/recovery behavior;
- determine whether sensor↔controller traffic needs cryptographic protection and whether the chosen sensor permits it.
