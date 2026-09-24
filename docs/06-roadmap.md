# Project Roadmap

## Phase 0 — Research and gap validation

### Goal

Prove that the narrowed OpenTouch product gap is real and identify at least one credible hardware path.

### Milestones

#### M0.1 — Freeze product thesis

**Done when:**

- product is explicitly defined as tiny / Linux-first / upstream / USB-C / preferably MOC;
- project documentation acknowledges existing Linux-first readers;
- FIDO2 biometric authenticators are explicitly separated from fprint/PAM readers.

**Status:** complete for initial research pass.

#### M0.2 — Resolve `3274:8012`

Determine:

- exact manufacturer/module identity;
- retail implementations;
- upstream driver path;
- released-driver availability;
- Fedora packaged availability;
- module documentation;
- sourcing;
- MOQ/pricing;
- security architecture;
- VID/PID policy.

**Exit criterion:** enough evidence to decide whether `3274:8012` is:
- only a proof-of-concept donor;
- a credible v1 production module;
- or a dead end.

**Status:** **NEXT ACTION.**

#### M0.3 — Identify second-source candidate

At least one independent alternative should be characterized well enough to avoid dependence on a single opaque vendor.

Candidates include NEXT, FocalTech, Goodix, ELAN, or another exact upstream-supported module.

### Phase-0 gate

Do not start a custom PCB until at least one candidate has:

- stable identity;
- viable sourcing;
- electrical/mechanical information;
- understood USB integration terms.

---

## Phase 1 — Legatum software prototype

### Goal

Prove the exact hardware works acceptably through the normal Fedora/fprint stack.

### Actions

- acquire exact known-VID:PID hardware;
- record USB descriptors;
- record Fedora/libfprint/fprintd versions;
- test packaged stack;
- test upstream stack if packaged support is missing;
- enroll;
- verify;
- GNOME Settings;
- GDM;
- lock screen;
- `sudo`;
- polkit;
- multi-user;
- unplug/replug;
- boot states;
- suspend/resume;
- repeated use;
- failure recovery.

### Artifacts

- `qualification/<device>/environment.md`
- `qualification/<device>/usb-descriptors.txt`
- `qualification/<device>/results.md`
- scripts needed to reproduce testing;
- issue log.

### Exit criterion

A device reaches **qualified prototype** state only if its observed behavior is good enough to justify physical product work.

---

## Phase 2 — Physical prototype

### Goal

Prove the desired physical product concept.

### Actions

- teardown donor if used;
- identify module markings;
- inspect PCB;
- model enclosure;
- create USB-C mechanical concept;
- prototype enclosure;
- validate cable/connector loads;
- confirm thermal/power behavior.

### Exit criterion

A compact enclosure can be built without damaging sensor reliability or making USB integration fragile.

---

## Phase 3 — Open hardware prototype

### Goal

Build a reproducible OpenTouch-specific hardware assembly.

### Actions

- schematic;
- PCB;
- USB-C integration;
- ESD protection;
- BOM;
- assembly files;
- enclosure CAD;
- programming/test fixtures if required;
- manufacturing notes;
- open-source licensing.

### Design rule

Keep the board minimal. Do not add a microcontroller without a demonstrated requirement.

### Exit criterion

Multiple units can be independently assembled from published artifacts and behave identically.

---

## Phase 4 — Upstream and distro integration

### Goal

Eliminate private software dependencies.

### Actions

Only as required:

- libfprint fixes;
- VID/PID additions;
- fprintd fixes;
- regression tests;
- distro packaging issues;
- documentation improvements.

### Exit criterion

Normal users can use the product through upstream/released distribution software on declared supported systems.

Private forks are not an acceptable product endpoint.

---

## Phase 5 — Productization

### Goal

Turn the open hardware prototype into a sellable small-batch device.

### Actions

- DFM;
- supplier agreement;
- production test fixture;
- incoming inspection;
- serial/revision tracking;
- packaging;
- compliance;
- labeling;
- warranty policy;
- support documentation;
- fulfillment;
- retail pricing;
- launch channel.

### Exit criterion

A production batch can be built, tested, shipped, and supported without engineering intervention per unit.

---

## Phase 6 — Optional keyboard integration

### Trigger

Only begin if:

- standalone OpenTouch is technically validated;
- sensor sourcing is stable;
- demand exists;
- integration value is clearer than simply attaching the standalone device.

The keyboard is explicitly not required for OpenTouch to be a meaningful contribution.

---

# Project-level gates

## Gate A — Sensor viability

Can we identify, buy, document, and legally integrate the sensor?

## Gate B — Linux viability

Does the exact hardware behave well through upstream Linux?

## Gate C — Product viability

Can a tiny, robust, consumer-quality physical design be made?

## Gate D — Economic viability

Can the unit be sold at a plausible price with sustainable margin?

## Gate E — Compliance viability

Can the product be sold in target markets without disproportionate certification/legal burden?

A failure at any gate should trigger architecture review rather than sunk-cost continuation.
