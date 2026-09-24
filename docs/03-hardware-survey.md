# Hardware Survey and Selection Rules

## 1. Selection philosophy

The product does not need the most sophisticated fingerprint silicon.

It needs hardware that is:

- small;
- reliable;
- sourceable;
- documented;
- electrically integrable;
- legally usable;
- stable across revisions;
- supported upstream;
- compatible with consumer product economics.

A technically excellent laptop sensor with no practical OEM channel is a poor product choice.

## 2. Current candidate table

This table is a research register, not a certification of compatibility.

| Candidate | USB ID | Architecture | Current relevance | Primary concern |
|---|---|---|---|---|
| Microarray / MAFP MOC | `3274:8012` | MOC | **Primary v0 lead** | Exact module identity, docs, sourcing, security details |
| NEXT NB-2020-U | `298d:2020` | USB OEM fingerprint module | Development/reference candidate | Larger than desired final form factor |
| NEXT NB-1010-U family | `298d:1010` | Fingerprint module | Secondary reference | Older ecosystem |
| FocalTech FT9365 ESS family | `2808:6553` | MOC | Technically interesting | OEM sourcing/docs |
| Goodix MOC families | various `27c6:*` | MOC | Broad technical relevance | SKU stability and procurement |
| ELAN ARM-M4 family | e.g. `04f3:0c9c` | MOC | Research candidate | Laptop/OEM sourcing |

Do not add “Fedora supported” to any row until the exact hardware has been tested or the exact packaged driver inclusion has been verified.

## 3. `3274:8012`

The official libfprint development supported-device list currently identifies:

```text
3274:8012  MAFP MOC Fingerprint Sensor
```

This establishes only:

- the exact USB ID appears in upstream development support;
- upstream categorizes it as an MOC fingerprint sensor.

It does **not** by itself establish:

- which retail products contain it;
- the exact underlying Microarray commercial module name;
- Fedora packaged support;
- module availability;
- template-storage guarantees;
- secure channel;
- liveness/PAD;
- stable firmware;
- OEM pricing;
- redistribution rights.

Those are the next research questions.

## 4. Why `3274:8012` leads v0

The device class appears to match the desired industrial design unusually well:

- compact;
- touch-style;
- MOC;
- native USB device;
- upstream Linux support exists at least at development level;
- commercial mini-readers reportedly expose this VID:PID.

That makes it a strong **proof-of-concept target** even if it ultimately fails as the production component.

## 5. NEXT NB-2020-U role

The NEXT NB-2020-U is interesting because OEM-oriented modules traditionally provide a clearer integration path than anonymous consumer dongles.

Its value may therefore be as:

- a reference platform;
- an independently supported second sensor;
- a debugging control;
- a comparison point for driver vs sensor problems.

It need not be the final product sensor.

## 6. Hardware evidence hierarchy

Use this order of confidence.

### Tier 1 — authoritative

- manufacturer datasheet;
- manufacturer product page;
- manufacturer engineering correspondence;
- upstream source/merge request;
- official USB descriptors collected from hardware.

### Tier 2 — strong secondary

- established distributor listing with manufacturer part number;
- teardown showing marked silicon;
- reproducible community hardware report with `lsusb`.

### Tier 3 — weak

- marketplace title;
- reseller description without part number;
- “works with Linux” claim;
- visual similarity;
- same enclosure as a known device.

OpenTouch must not base production decisions on Tier-3 evidence.

## 7. Required fields before selecting a v1 sensor

For each serious candidate collect:

- manufacturer;
- exact manufacturer part number;
- module/sensor name;
- USB VID:PID;
- USB class/protocol behavior;
- MOC vs host matching;
- physical dimensions;
- active sensing area;
- supply voltage/current;
- connector/pinout;
- datasheet;
- mechanical drawing;
- lifecycle status;
- firmware update mechanism;
- template storage model;
- raw-image exposure;
- secure-channel behavior;
- PAD/liveness claims;
- upstream libfprint driver;
- first supported libfprint version;
- Fedora packaged support;
- sample pricing;
- 10 / 100 / 1k pricing;
- MOQ;
- lead time;
- authorized sourcing channel;
- VID/PID integration terms;
- NDA requirements;
- redistribution restrictions.

## 8. Product architecture preference

Preferred v1 architecture:

```text
OEM MOC fingerprint module
        ↓ native USB if available
minimal carrier PCB
        ↓
USB-C + CC resistors + ESD/power protection
        ↓
host
```

Avoid adding an MCU unless there is a demonstrated requirement.

Every additional controller introduces:

- firmware;
- update mechanisms;
- attack surface;
- USB identity complexity;
- manufacturing complexity;
- new failure modes.

## 9. Donor-reader warning

A donor product is acceptable for Phase 1.

A donor product is **not automatically a production supply chain**.

Before designing around its internal module, determine whether:

- the module is independently orderable;
- it has a stable part number;
- the manufacturer permits OEM integration;
- its firmware is not tied to the donor vendor;
- continued use of its USB identity is permitted.

## Sources

- libfprint supported devices: https://fprint.freedesktop.org/supported-devices.html
