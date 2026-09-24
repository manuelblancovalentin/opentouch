# OpenTouch Architecture Decision Records

This directory is the authoritative ledger of architectural and product decisions for OpenTouch.

An Architecture Decision Record (ADR) captures **what was decided and why**. It is not a research notebook and not an action item.

## Decision states

Use:

- `Proposed` — a concrete decision is under consideration;
- `Accepted` — currently authoritative;
- `Superseded` — replaced by a later ADR;
- `Rejected` — considered and deliberately not adopted;
- `Deprecated` — still historically relevant but no longer recommended.

Do not delete old accepted ADRs when the project changes direction. Mark them `Superseded` and link to the replacement. The history is part of the engineering record.

## Decision ledger

| ADR | Decision | Status | Date |
|---|---|---|---|
| [ADR-0001](ADR-0001-raw-sensor-opentouch-controller.md) | Adopt raw commercial sensor + OpenTouch-owned controller architecture | Accepted | 2026-09-24 |

## Likely upcoming decisions

These are **not yet ADRs**. They are listed only so the expected decision sequence is visible.

- Development fingerprint sensor selection
- Controller / MCU / SoC selection
- Template-storage architecture
- Device portability vs host binding
- Production biometric-matching architecture
- Host/device protocol security model

Create an ADR only when there is a real alternative to evaluate and enough evidence to make or formally propose a decision.

## Naming

Use:

```text
ADR-NNNN-short-description.md
```

Examples:

```text
ADR-0001-raw-sensor-opentouch-controller.md
ADR-0002-development-sensor-selection.md
ADR-0003-controller-selection.md
```

Numbers are permanent and never reused.

## Required ADR structure

Every ADR should contain:

```markdown
# ADR-NNNN — Title

- Status:
- Date:
- Related actions:
- Supersedes:
- Superseded by:

## Context

## Decision

## Alternatives Considered

## Rationale

## Consequences

### Positive

### Negative

## Assumptions

## Open Questions

## Revisit Triggers

## Evidence
```

Not every section must be long. The purpose is to preserve enough reasoning that the decision remains intelligible months or years later.

## Relationship to action records

The normal flow is:

```text
research
   ↓
OT action / investigation
   ↓
evidence
   ↓
ADR
   ↓
new actions
```

Example:

```text
OT-0001
Raw sensor feasibility scan
        ↓
candidate measurements / vendor responses
        ↓
ADR-0002
Select development sensor
        ↓
OT-0006
Capture first raw fingerprint frame
```

This separation prevents project history from becoming ambiguous:

- research says **what we know**;
- OT records say **what we need to do**;
- ADRs say **what we decided**.
