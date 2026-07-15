# Design Principles

## Purpose

Read this file for every request. It defines the skill's baseline worldview.

## Harness-first principles

1. Treat the loop as the product surface.
2. Make tool execution, memory, policy, and recovery explicit.
3. Keep state inspectable, durable, and easy to debug.
4. Add complexity only when task shape creates pressure for it.
5. Keep governance centralized when side effects or autonomy increase.
6. Require an evaluation and recovery story before trusting long-running
   behavior.

## Design posture

- Prefer minimal viable harnesses before control-plane expansion.
- Prefer specific architecture decisions over broad agent rhetoric.
- Prefer explicit artifacts over hidden prompt state.
- Prefer reversible choices in early versions.

## What this skill must do

- ask for missing architectural facts
- challenge weak assumptions
- map the design to known harness patterns
- explain why a pattern is chosen
- choose a primary family before opening multiple branches
- make mixed-family priority explicit when hybrid pressure exists

## Output discipline

- Output sections should follow the activated branches and chosen subsystems,
  not a fixed universal checklist.
- Always include family read, runtime story, deferred complexity, and pattern
  provenance.
- Include `Design Review Checklist` only for heavier or riskier designs.
