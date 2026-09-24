# Action Register

This file is the ordered working queue.

Statuses:

- `TODO`
- `IN PROGRESS`
- `BLOCKED`
- `DONE`
- `DROPPED`

Priorities:

- `P0` — blocks current project decision;
- `P1` — next-stage critical;
- `P2` — useful but non-blocking;
- `P3` — later.

---

## OT-0001 — Resolve the exact `3274:8012` hardware

**Priority:** P0  
**Status:** TODO  
**Phase:** 0

### Question

What exact Microarray/MAFP sensor or module enumerates as USB `3274:8012`, and can it support an actual OpenTouch product?

### Required outputs

#### Identity

- manufacturer legal name;
- sensor/module part number;
- module marking;
- USB descriptor strings;
- VID owner;
- PID assignment;
- photographs/teardowns;
- known retail products using the exact ID.

#### Linux

- libfprint driver name/path;
- commit/MR adding support;
- merge date;
- first release containing support;
- Fedora 43/44/45 package inclusion;
- known bugs;
- fprintd behavior;
- template-storage behavior exposed through libfprint.

#### Electrical/mechanical

- dimensions;
- interface;
- voltage/current;
- connector;
- pinout;
- sensor active area;
- carrier requirements;
- ESD requirements.

#### Security

- MOC confirmation from primary source;
- template location;
- template exportability;
- raw-image exposure;
- firmware update/security model;
- USB transport authentication/encryption;
- replay protections;
- PAD/liveness claims.

#### Sourcing

- manufacturer sales contact;
- sample availability;
- distributor(s);
- MOQ;
- 10 / 100 / 1k pricing;
- lead time;
- lifecycle status;
- NDA requirements;
- production availability.

#### Product/legal

- can an OEM ship the complete USB module under `3274:8012`?
- would OpenTouch need its own VID/PID?
- can firmware be redistributed?
- are vendor binaries required?
- are there branding/licensing constraints?

### Decision

At completion classify it as one of:

- `V0_ONLY` — useful donor/proof-of-concept, unsuitable for production;
- `V1_CANDIDATE` — credible production path;
- `REJECTED` — do not build around it.

---

## OT-0002 — Acquire a verified `3274:8012` donor

**Priority:** P0  
**Status:** BLOCKED by OT-0001 product identification  
**Phase:** 1

Requirements:

- seller or independent evidence shows exact VID:PID;
- avoid listings identified only by enclosure/photo;
- retain invoice/listing/revision evidence.

After arrival:

```bash
lsusb
lsusb -v -d 3274:8012
```

Archive descriptors.

---

## OT-0003 — Establish Fedora baseline

**Priority:** P0  
**Status:** TODO after hardware acquisition  
**Phase:** 1

Record:

```bash
cat /etc/fedora-release
rpm -q libfprint fprintd
fprintd-list "$USER"
```

Test stock packaged stack before installing anything custom.

If stock Fedora fails, distinguish:

- unsupported packaged version;
- permissions/policy issue;
- driver bug;
- device/firmware issue.

---

## OT-0004 — Test current upstream libfprint

**Priority:** P0  
**Status:** TODO if stock package fails or for comparison  
**Phase:** 1

Goal:

Determine whether current upstream support actually works on the acquired exact hardware.

Do not replace the system stack blindly. Use a controlled development/test environment where practical.

Record:

- exact commit;
- build options;
- detection;
- enrollment;
- verify;
- error logs.

---

## OT-0005 — Run functional qualification

**Priority:** P1  
**Status:** TODO  
**Phase:** 1

Test:

- enrollment;
- repeated verification;
- GNOME Settings;
- GDM;
- GNOME lock screen;
- `sudo`;
- polkit;
- password fallback;
- failed fingerprint;
- multi-user;
- hotplug;
- boot attached/detached;
- suspend/resume;
- service restart.

Create a reproducible result table.

---

## OT-0006 — Run preliminary reliability campaign

**Priority:** P1  
**Status:** TODO  
**Phase:** 1

Collect enough observations to compare usage quality, not to claim formal biometric FAR/FRR.

Record:

- touch latency;
- retries;
- failures;
- finger orientation;
- dry/damp finger;
- post-resume first touch;
- long-idle first touch.

---

## OT-0007 — Teardown donor

**Priority:** P1  
**Status:** TODO after initial qualification  
**Phase:** 2

Goals:

- photograph PCB;
- record all markings;
- identify sensor/module;
- identify controllers/passives;
- determine whether module is native USB;
- identify connector/pinout if possible;
- compare with vendor information.

Preserve the donor if destructive teardown is unnecessary.

---

## OT-0008 — Contact Microarray

**Priority:** P0/P1  
**Status:** TODO as soon as exact identity is sufficient  
**Phase:** 0

Request:

- exact part number;
- datasheet;
- mechanical drawing;
- electrical interface;
- sample;
- MOQ;
- tier pricing;
- lead time;
- lifecycle;
- MOC architecture documentation;
- template-storage/security documentation;
- firmware policy;
- USB VID/PID OEM policy;
- Linux/open-source integration permission;
- NDA requirements.

---

## OT-0009 — Characterize second-source candidate

**Priority:** P1  
**Status:** TODO  
**Phase:** 0

Goal:

Avoid making the entire project dependent on Microarray.

Preferred output: one alternative module with exact part number, upstream path, samples, documentation, and pricing.

---

## OT-0010 — Decide v1 architecture

**Priority:** P0 gate  
**Status:** BLOCKED by OT-0001, OT-0005, OT-0008, OT-0009  
**Phase:** 2/3

Decision document must answer:

- chosen module;
- source;
- why;
- native USB vs intermediate MCU;
- USB identity;
- template/security model;
- target BOM;
- fallback supplier.

Only after this action is complete should a custom OpenTouch PCB become the primary engineering task.

---

# Immediate next session

Start with **OT-0001**.

The goal is not yet to purchase “something that probably contains Microarray.”

The goal is to establish the exact identity and supply chain of `3274:8012` strongly enough that the first purchase is an instrumented engineering decision rather than a marketplace guess.
