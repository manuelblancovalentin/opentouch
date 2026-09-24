# Security Model

## 1. Security posture

OpenTouch must not use “match-on-chip” as a synonym for “secure.”

MOC is an architectural property. Security depends on additional details:

- template storage;
- raw-image handling;
- reader firmware trust;
- transport authentication;
- replay resistance;
- enrollment authorization;
- presentation-attack resistance;
- host behavior;
- fallback policy.

Claims must follow evidence.

## 2. Assets

Potentially sensitive assets:

- fingerprint images;
- fingerprint templates;
- template identifiers/handles;
- enrollment metadata;
- user-to-template mapping;
- authentication results;
- device firmware;
- host credentials unlocked by successful PAM authentication.

## 3. Adversaries

At minimum consider:

- malicious local user;
- malicious root/host;
- malicious USB host;
- malicious reader firmware;
- attacker with a stolen reader;
- attacker presenting spoofed biometric material;
- attacker replaying captured USB traffic;
- supply-chain attacker;
- unauthorized enrollee.

## 4. Threats

### 4.1 Raw biometric exposure

Questions:

- Does the reader expose raw images?
- Can a privileged host request raw images?
- Does enrollment stream raw biometric data to the host?
- Are debug commands capable of extracting images?

For a product marketed as MOC, the desired result is that normal operation does not require host-side raw-image processing.

### 4.2 Template storage

Determine:

- device-side vs host-side;
- template encryption;
- template exportability;
- per-device keying;
- erase semantics;
- maximum template count;
- multi-user mapping.

“Stored on chip” does not establish that storage is encrypted or non-exportable.

### 4.3 Spoofing / presentation attacks

Fingerprint authentication is vulnerable to presentation attacks unless the sensor implements effective PAD/liveness mechanisms.

Do not claim liveness unless the exact module has documented and preferably independently validated PAD behavior.

### 4.4 Replay

If the host-reader protocol merely reports “match yes/no” without authenticated freshness, traffic replay may be relevant.

Questions:

- challenge/response?
- session nonce?
- authenticated channel?
- encrypted transport?
- host authentication?
- reader authentication?

### 4.5 Malicious reader firmware

A compromised reader could simply report successful matches.

Questions:

- is firmware signed?
- is firmware updateable?
- who controls signing?
- can host downgrade firmware?
- is firmware version readable?
- can production units be locked?

### 4.6 Stolen device

If templates are stored on-device:

- can they be extracted?
- can the device be reset?
- does physical possession enable offline attacks?
- are templates useful outside that device?

### 4.7 Enrollment authorization

Enrollment must not become an unprivileged path to add an attacker's finger.

Qualification must inspect:

- fprintd policy;
- polkit behavior;
- local user permissions;
- administrative enrollment for another account.

### 4.8 PAM fallback

Password fallback is necessary operationally but changes security behavior.

Document:

- timeout;
- max fingerprint attempts;
- whether password can be entered immediately;
- behavior when reader is absent;
- behavior when fprintd fails.

## 5. Architecture comparison

| Property | Host matching | MOC | MOC + authenticated secure channel | FIDO2 biometric authenticator |
|---|---|---|---|---|
| Matching location | Host | Reader | Reader | Authenticator |
| Raw images normally required by host | Often | Ideally no | Ideally no | No for normal FIDO operation |
| Template location | Often host | Usually reader | Reader | Authenticator |
| Replay resistance | Protocol-dependent | Protocol-dependent | Better if designed correctly | Challenge-based FIDO protocol |
| PAM/fprint native | Yes | Yes | Yes | Not inherently |
| Phishing-resistant web authentication | No | No | No | Yes, if used as FIDO |
| “Secure because biometric” | No | No | No | Still requires correct deployment |

OpenTouch and a biometric FIDO key solve different problems.

## 6. Security claims policy

Allowed only when verified:

- “match-on-chip”;
- “templates remain on the reader during normal operation”;
- “no proprietary host driver”;
- “encrypted/authenticated transport”;
- “firmware signature verification”;
- “PAD/liveness support.”

Prohibited without strong evidence:

- “Touch ID-level security”;
- “unhackable”;
- “spoof-proof”;
- “secure element”;
- “biometric data can never leave the device”;
- “anti-replay”;
- “enterprise-grade security.”

## 7. Preferred privacy architecture

OpenTouch the company should not receive biometric information.

Preferred model:

```text
user
 ↓
local reader ↔ local Linux host
```

No OpenTouch account.
No biometric cloud.
No fingerprint telemetry.
No remote template backup.

This minimizes both privacy risk and legal exposure.

## 8. `3274:8012` security questions

Before production selection, obtain answers to:

1. Where are enrolled templates stored?
2. Are templates encrypted at rest?
3. Can templates be exported?
4. Does the host ever receive raw fingerprint images?
5. Is USB traffic encrypted?
6. Is it authenticated?
7. Is replay mitigated?
8. Does the sensor implement PAD/liveness?
9. Is firmware signed?
10. Can firmware be updated or downgraded?
11. Can a malicious host force template export/debug mode?
12. Can the module be securely factory-reset?
