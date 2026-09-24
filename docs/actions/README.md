# OpenTouch Action Records

This directory contains the authoritative execution records for OpenTouch work items.

An action record describes **work to be performed, evidence to collect, and completion criteria**.

It is distinct from:

- research documentation — what we know;
- ADRs — what we decided;
- `STATUS.md` — where the project currently stands.

## Action states

Use:

- `TODO` — defined but not started;
- `IN PROGRESS` — active work;
- `BLOCKED` — cannot proceed until a dependency is resolved;
- `DONE` — completion criteria satisfied and evidence recorded;
- `DROPPED` — intentionally abandoned.

Completed action records should remain in the repository as engineering history. They may later be moved to an archive/completed subdirectory if the project adopts that convention, but their identifiers must never be reused.

## Priority

Use:

- `P0` — blocks the current project decision or critical path;
- `P1` — next-stage critical;
- `P2` — useful but non-blocking;
- `P3` — later / optional.

## Action ledger

| Action | Description | Priority | Status | Related ADR |
|---|---|---:|---|---|
| [OT-0001](OT-0001-raw-sensor-feasibility-scan.md) | Raw fingerprint sensor feasibility scan | P0 | IN PROGRESS | ADR-0001 |

## Naming

Use:

```text
OT-NNNN-short-description.md
```

Examples:

```text
OT-0001-raw-sensor-feasibility-scan.md
OT-0002-select-development-sensor.md
OT-0003-select-controller-platform.md
```

Action numbers are permanent and never reused.

## Required action structure

Each action should normally contain:

```markdown
# OT-NNNN — Title

- Status:
- Priority:
- Phase:
- Opened:
- Closed:
- Related ADRs:
- Depends on:
- Blocks:

## Objective

## Why This Exists

## Scope

## Required Evidence

## Work Plan

## Deliverables

## Completion Criteria

## Decision Output

## Findings

## Result

## Follow-up Actions
```

Not every action needs every section populated at creation time. `Findings`, `Result`, and `Follow-up Actions` are expected to evolve while the action is active.

## Relationship to ADRs

Typical flow:

```text
research question
      ↓
OT action
      ↓
evidence / experiments / vendor responses
      ↓
ADR
      ↓
new OT actions
```

An OT record should not silently become a decision record.

If an action results in an architectural or product decision, create or update the appropriate ADR and link both records.
