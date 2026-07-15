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
- shared session or control-plane abstraction if multiple surfaces exist
- provider-compat or structured-output contract if stages depend on it

## 4. Activated Pressure Ledger

List activated branches only, in priority order.

For each branch include:

- pressure that triggered it
- decision made
- borrowed methods
- heavier pattern rejected for now

List closed branches only if they were tempting but unjustified.

Add a short `Checked but Closed` list only for branches that were examined
briefly, then intentionally left closed.

## 5. Activated Subsystem Specs

Create one subsection per activated subsystem. Example headings:

- `Governance and permissions`
- `Memory and durable facts`
- `Gateway and session reinjection`
- `Delegation and child ownership`
- `Provider and fallback strategy`
- `State artifacts and recovery`
- `Capability supply and control surfaces`
- `Background tasks and unattended automation`

Include only justified subsections. For each one state:

- why the subsystem exists
- what concrete mechanism is used
- what project methods it borrows
- what was intentionally left out

## 6. State, Session, and Recovery Story

- durable artifacts
- session continuity model
- session keys, scope, and freshness rules if sessions span channels or workers
- checkpoint or staged-resume model if the runtime is graph-like or long-lived
- compaction or context-reset behavior if relevant
- pre-turn and post-turn memory lifecycle if memory is host-managed
- restart, interruption, and recovery rules
- task/team approval protocol if background workers or swarms exist
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
