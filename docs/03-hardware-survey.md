# Raw Sensor Survey and Selection Rules

## 1. New selection objective

OpenTouch no longer seeks a complete USB MOC module as its primary architecture.

The target component is a **commercial fingerprint sensing IC/module** that exposes fingerprint data through a sufficiently low-level interface such as SPI.

The OpenTouch controller will own the authentication architecture above that sensor.

## 2. Candidate classes

### Fingerprint Cards access sensors

The FPC1020 / FPC1024 / FPC1025 family is particularly relevant because published material describes:

- capacitive sensing;
- 508 dpi;
- grayscale image output;
- SPI interface;
- 1.8 V / 3.3 V operation;
- compact square/round packages intended for embedded access applications.

These are currently strong Phase-0A candidates.

### Goodix capacitive SPI sensors

Goodix currently lists several capacitive fingerprint sensors with SPI interfaces and compact form factors.

Representative candidates include:

- GF3988;
- GF5288;
- GF3258;
- GF3626.

Exact raw-image access, register documentation, SDK requirements, redistribution terms, and engineering-sample availability must be verified before any selection.

## 3. Sensor selection gate

A candidate may only become the OpenTouch sensor if all of the following are answered.

### Technical

- exact part number;
- sensor resolution;
- active area;
- image format;
- host interface;
- timing;
- voltage/current;
- reset/interrupt behavior;
- calibration requirements;
- ESD constraints;
- cover/coating constraints;
- documented raw or low-level acquisition path.

### Openness

- can OpenTouch initialize the sensor without a proprietary host daemon?
- can acquisition firmware be independently written?
- are register/interface docs available?
- is a binary blob required?
- may OpenTouch publish source code for the interface?
- do NDA terms prohibit an open implementation?

### Sourcing

- prototype samples;
- distributor;
- MOQ;
- 10 / 100 / 1k pricing;
- lead time;
- lifecycle status;
- PCN/EOL policy.

### Security

The raw sensor itself need not store templates.

Instead determine whether it has:

- unique identity/key material;
- authenticated transport capability;
- firmware;
- sensor pairing requirements;
- anti-tamper or PAD features.

OpenTouch must not assume any of these.

## 4. Hardware architecture preference

Initial development:

```text
raw fingerprint sensor
        ↓ SPI
OpenTouch controller
        ↓ USB-C
Linux
```

The first controller prototype may use host-side matching to de-risk acquisition.

Target production architecture:

```text
raw fingerprint sensor
        ↓
OpenTouch controller
        ├── preprocessing
        ├── template generation
        ├── matching
        ├── secure template storage
        ├── secure boot
        ├── signed updates
        └── authenticated host protocol
        ↓
USB-C
        ↓
Linux
```

## 5. Controller requirements

The controller choice should eventually be evaluated for:

- sufficient RAM for fingerprint image processing;
- hardware cryptography;
- protected key storage;
- secure boot;
- signed firmware update;
- anti-rollback support;
- USB device support;
- SPI bandwidth;
- flash capacity;
- debugging lockout;
- cost;
- package manufacturability.

No MCU/SoC should be selected before the sensor data size and biometric-processing requirements are known.

## 6. Benchmark device

`3274:8012` remains a useful benchmark.

It should be treated as:

```text
REFERENCE_DEVICE
```

not:

```text
TARGET_SENSOR
```

We may still acquire one to measure:

- time-to-enroll;
- time-to-match;
- physical UX;
- Fedora integration;
- suspend/resume;
- failure behavior;
- device-side template semantics.

## 7. Phase-0A decision

The immediate decision is:

> Can we acquire a raw sensor with enough documentation and legal freedom to implement an open controller stack?

If yes, proceed custom.

If no candidate satisfies this after serious vendor outreach, reconsider using an integrated MOC module.
