# Harness Architecture Spec

## 1. Agent Summary

- mission
- primary user
- success condition
- failure condition

## 2. Family Detection

- primary family
- secondary pressures
- rejected families
- why this read is strongest

## 3. Runtime Shape and Loop

- chosen shape
- turn structure
- state transitions
- event sources
- plan/build or other mode splits if any

## 4. Activated Pressure Ledger

List only the branches that actually mattered, in priority order.

For each activated branch include:

- pressure that triggered it
- decision made
- borrowed methods
- heavier pattern rejected for now

Then list closed branches only if they were tempting but unjustified.

Also include a short `Checked but Closed` list when a branch was examined
briefly but intentionally not activated further.

## 5. Activated Subsystem Specs

Create one subsection per activated subsystem, for example:

- `Governance and permissions`
- `Memory and durable facts`
- `Gateway and session reinjection`
- `Delegation and child ownership`
- `Provider and fallback strategy`
- `Background tasks and recovery`

Only include the subsections justified by the design. For each subsection
state:

- why the subsystem exists
- what concrete mechanism is used
- what project methods it borrows
- what was intentionally left out

## 6. State, Session, and Recovery Story

- durable artifacts
- session continuity model
- compaction or context-reset behavior if relevant
- restart, interruption, and recovery rules
- inspectability and operator visibility

## 7. Evaluation and Failure Checks

- realistic tasks
- expected failure modes
- operator checks
- what must be proven before autonomy is widened

## 8. Phased Roadmap

- v1 minimal harness
- v2 justified expansions

## 9. Pattern Provenance

- link to `Pattern Provenance Matrix`
- summarize the most important borrowed and rejected methods
