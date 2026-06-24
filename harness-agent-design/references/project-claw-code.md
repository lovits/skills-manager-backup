# Claw Code Notes

## Why This Project Matters

claw-code is a strong source for coding harnesses with centralized runtime
governance. Its value is not only "coding agent" but the way it keeps
permissions, hooks, policy, session control, recovery, and task freshness in
one runtime-owned layer.

Its codegraph clusters around:

- `rust/crates/runtime/src/config.rs`
- `rust/crates/runtime/src/policy_engine.rs`
- `rust/crates/runtime/src/hooks.rs`
- `rust/crates/runtime/src/session_control.rs`
- `rust/crates/runtime/src/recovery_recipes.rs`
- `rust/crates/runtime/src/task_registry.rs`

## Subsystem Methods

### Centralized runtime governance

Borrow:

- one runtime feature config that owns hooks, plugins, MCP, provider fallback,
  permission mode, rules import, and sandbox posture
- config inspection and precedence reporting instead of hidden runtime state

Use when:

- the user needs one authority for execution posture

### Policy engine above tools

Borrow:

- policy conditions and actions as explicit data, not scattered `if` blocks
- lane context including green level, freshness, blockers, review status,
  approval token, and retry state
- policy decisions that can retry, escalate, reconcile, merge, or require
  approval

Use when:

- the runtime needs auditable decisions about what may happen next

Avoid when:

- a lightweight harness only needs simple approvals

### Hooks as governed interception

Borrow:

- pre-tool and post-tool hooks with progress reporting
- hook-level permission overrides and revised tool input
- abortable hook execution with explicit started/completed/cancelled events

Use when:

- policy should intervene before or after tools without moving logic into each
  tool implementation

### Session control and namespace discipline

Borrow:

- per-workspace session stores
- session reference aliases such as latest, with exclusion of the current
  empty session
- managed session paths and workspace fingerprints to avoid collisions

Use when:

- session resume must be safe across multiple workspaces or processes

### Recovery recipes

Borrow:

- named failure scenarios with one automatic recovery pass before escalation
- recovery ledgers that track attempts, remaining budget, and escalation
  reason
- different recipes for trust prompts, stale branches, MCP failures, plugin
  startup, provider failures, and prompt misdelivery

Use when:

- the harness is long-running enough that known failures need first-class
  recovery logic

### Task registry and lane freshness

Borrow:

- explicit task lifecycle states
- heartbeat-based freshness for running lanes
- generated lane boards separating active, blocked, and finished work

Use when:

- subagent or worker tasks need operational visibility

## Question Angles This Project Justifies

- Should governance live centrally in the runtime, or is it leaking into
  tools?
- Does this system need policy conditions and recoveries, or only simple
  approvals?
- What failures deserve one automatic recipe before escalation?
- If there are child lanes, how will freshness and blocked state be observed?

## Design Warnings

- Do not import all of claw-code's governance machinery into a simple chat or
  research agent.
- Do not scatter policy across tools if the runtime can own it once.
