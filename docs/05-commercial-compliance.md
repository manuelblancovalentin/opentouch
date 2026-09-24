# Commercial Model and Compliance

## 1. Open-source boundary

The commercial model should not depend on closing the Linux integration stack.

Recommended boundary:

| Asset | Default |
|---|---|
| setup/diagnostic software | Open |
| upstream driver patches | Upstream/open |
| qualification suite | Open |
| PCB/schematic | Open |
| BOM | Open |
| enclosure CAD | Open |
| user documentation | Open |
| vendor firmware | Vendor terms |
| OpenTouch trademark | Controlled |
| supplier contracts/pricing | Private |
| internal production QA details | May remain private |
| factory relationship | Private |

Potential licenses:

- software: Apache-2.0, MIT, or GPL-family depending on code purpose/upstream constraints;
- hardware: CERN-OHL-P-2.0 or CERN-OHL-S-2.0;
- documentation: CC BY 4.0 or CC BY-SA 4.0;
- CAD: hardware license or CC-family depending on desired reciprocity.

Final choices should be made before accepting external contributions.

## 2. Commercial moat

Potential defensible value:

- reliable supplier;
- stable hardware revision;
- qualification;
- enclosure quality;
- assembly quality;
- tested firmware;
- distro compatibility matrix;
- support;
- fulfillment;
- warranty;
- brand.

An open PCB does not eliminate those advantages.

## 3. Business models

### 3.1 Open hardware + assembled product

Strong fit.

Users can inspect/reproduce the design while most customers buy a finished device.

### 3.2 Open software + closed hardware

Possible, but weaker alignment with the project's stated open-hardware objective.

May become necessary if vendor agreements prevent publication of critical module details.

### 3.3 Open design + premium finished product

Potentially strongest model if the object is mechanically polished.

### 3.4 Kits

Useful for developers, but a fingerprint sensor may be sufficiently integrated that the kit offers little advantage over a completed device.

### 3.5 Small-batch direct sales

Likely first commercial path.

### 3.6 Crowd Supply / crowdfunding

Potential fit after:

- functional prototype;
- sourcing agreement;
- stable BOM;
- manufacturing quote;
- compliance plan.

Do not use crowdfunding to discover whether the hardware can actually be sourced.

### 3.7 Keyboard integration

Deferred.

The standalone reader should validate the core technology and market before creating a much larger keyboard project.

## 4. Preliminary economics

These are sensitivity ranges, not vendor quotes.

| Item | Prototype/tens | 500–2,000 units |
|---|---:|---:|
| fingerprint module | $15–50+ | target assumption: $8–25 |
| PCB + USB-C + ESD/passives | $8–20 | $2–6 |
| enclosure | $5–20 | $2–8 |
| assembly/test | $5–15 | $2–7 |
| packaging | $2–6 | $1.50–4 |
| scrap/warranty allowance | $2–8 | $1–5 |
| approximate landed hardware COGS | $37–119 | ~$17–55 |

The sensor cost is currently the dominant unknown.

A preliminary consumer price band worth testing is roughly:

- **$69–99** target range;
- perhaps ~$109 for an early low-volume enthusiast unit.

A product requiring a much higher retail price should be re-evaluated against the size of the Linux desktop market.

## 5. Market anchors

Relevant comparison points include:

- ThinkPenguin's Linux-oriented external reader;
- commodity Windows Hello fingerprint dongles;
- enterprise fingerprint readers;
- YubiKey C Bio at approximately $98 as a premium biometric USB-C security device.

These are not direct substitutes, but they bound user expectations.

## 6. Compliance categories

### 6.1 One-off personal prototype

Primary concerns:

- electrical safety;
- avoiding hardware damage;
- correct USB design.

Commercial certification is not the Phase-1 blocker.

### 6.2 Open-source DIY design / kit

Additional concerns:

- component safety;
- claims/disclaimers;
- product-liability structure;
- whether the seller is placing a complete electronic product on the market.

Exact obligations depend on what is sold and where.

### 6.3 U.S. small-batch commercial product

Likely topics:

- FCC Part 15 / unintentional-radiator obligations;
- supplier declaration/certification pathway as applicable;
- product labeling;
- USB trademark/compliance rules if USB marks are used;
- state privacy law;
- warranty/product liability;
- import/customs;
- applicable materials restrictions through supply chain/customer requirements.

### 6.4 EU commercial product

Likely topics:

- CE/EMC;
- RoHS;
- WEEE;
- product traceability;
- importer/manufacturer obligations;
- documentation/technical file.

Obtain specialist compliance advice before launch.

## 7. USB identity

A critical productization question is ownership/use of USB VID:PID.

Do not assume that integrating an OEM sensor allows OpenTouch to freely ship under the sensor vendor's VID/PID.

Before custom hardware:

- ask the sensor vendor about OEM enumeration policy;
- determine whether the module remains a complete USB device;
- determine whether OpenTouch needs its own VID/PID;
- determine how a new hardware ID affects libfprint;
- understand USB-IF trademark/logo conditions.

Do not invent or reuse a VID without authorization.

## 8. Biometric privacy

The preferred architecture is local-only:

- no biometric cloud;
- no OpenTouch fingerprint database;
- no template synchronization service;
- no biometric analytics telemetry.

Illinois BIPA and other biometric/privacy regimes are a reason to keep OpenTouch out of the biometric-data custody path wherever possible.

This is not legal advice; commercial launch needs jurisdiction-specific review.

## 9. Commercial kill criteria

Reconsider the product if:

- stable sensor cost cannot reach the target range;
- MOQ requires unjustifiable inventory;
- compliance cost dominates small-batch economics;
- vendor terms prohibit a reproducible open design;
- support costs imply enterprise-level pricing;
- the market converges on cheap officially Linux-supported mini readers before launch.

## Sources

- ThinkPenguin TPE-F4500: https://www.thinkpenguin.com/gnu-linux/optical-usb-fingerprint-reader-gnulinux-edition-tpe-f4500
- YubiKey C Bio: https://www.yubico.com/product/yubikey-c-bio/
- USB-IF developer information: https://www.usb.org/developers
- USB-IF logo licensing: https://www.usb.org/logo-license
- Illinois BIPA statute: https://www.ilga.gov/legislation/ilcs/fulltext.asp?DocName=074000140K10


## 10. Revised commercial architecture

OpenTouch has selected the raw-sensor/custom-controller direction as its target architecture.

Commercial implications:

### Advantages

- stronger technical differentiation;
- reusable controller/platform across multiple products;
- less dependence on one opaque USB MOC supplier;
- ability to second-source sensing ICs behind an OpenTouch abstraction;
- control over security/data architecture;
- stronger open-source/community contribution;
- potential future OEM/reference-design value.

### Costs

- longer development cycle;
- higher firmware/software effort;
- biometric algorithm qualification;
- controller/secure-storage complexity;
- new libfprint integration;
- higher engineering risk.

Near-term revenue may be worse than simply packaging an existing MOC module. The decision is justified by long-term platform value rather than minimum time-to-market.

A benchmark `3274:8012` unit may still be purchased for comparison.
