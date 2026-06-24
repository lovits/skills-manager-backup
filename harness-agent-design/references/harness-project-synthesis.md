# Harness Project Synthesis

## Purpose

Use this file at the start of every request.

This is not a catalog of full-project templates. It is a codegraph-backed
method atlas for deciding:

- which harness family is most likely
- which branch to open next
- which subsystem methods to borrow
- which heavier project shape to reject for now

## Local Method Sources

- `/Users/qianye/Downloads/找工作/DL/claw-code`
- `/Users/qianye/Downloads/找工作/DL/opencode`
- `/Users/qianye/Downloads/找工作/DL/openclaw`
- `/Users/qianye/Downloads/找工作/DL/OpenHarness`
- `/Users/qianye/Downloads/找工作/DL/OpenHarness/ohmo`
- `/Users/qianye/Downloads/找工作/DL/hermes-agent`
- `/Users/qianye/Downloads/找工作/DL/nanobot`
- `/Users/qianye/Downloads/找工作/DL/learn-claude-code`

## Family Detection Map

### Coding harness

Signals:

- edits code, runs shell, verifies results
- approval posture matters
- plan/build split or owner/helper split may matter

Lean first on:

- `opencode` for mode split, prompt admission, and child-session boundary
- `claw-code` for central governance, policy, recovery, and task freshness
- `learn-claude-code` for mechanism-first escalation from simple loop upward

### Always-on or gateway harness

Signals:

- external events, multiple channels, background work
- restart/resume and session reinjection matter
- operator inspectability matters

Lean first on:

- `OpenClaw` for gateway control plane, delegated runs, native hook relay, and
  fallback chains
- `OpenHarness` plus `ohmo` for reusable core plus channel-facing overlay,
  session-key runtime reuse, and snapshot restore
- `hermes-agent` when restart-resume and remote daemon behavior are central

### Providerized shared-core harness

Signals:

- more than one backend is truly in play
- memory, MCP, auxiliary work, or surfaces may swap providers
- one shared core should serve CLI and remote surfaces

Lean first on:

- `hermes-agent` for providerized subsystems and auxiliary fallback routing
- `OpenClaw` for runtime fallback chains across provider/model candidates
- `OpenHarness` when the shared core is an assembly platform, not just provider
  abstraction

### Lightweight or teaching harness

Signals:

- the risk is overbuilding
- the user needs the smallest legible loop first
- the agent may never need gateways, provider layers, or multi-agent runtime

Lean first on:

- `nanobot` for the smallest useful loop
- `learn-claude-code` for staged mechanism growth

## Subsystem Borrowing Matrix

### Loop and mode design

- `opencode`: `plan` versus `build`, visible phase changes, admitted input
  promotion, primary loop plus narrow helper
- `claw-code`: one owner loop with runtime-owned governance and recovery
- `OpenHarness`: reusable tool cycle plus background task lifecycle
- `OpenClaw`: shared session abstraction across interactive, print, and RPC
- `nanobot`: minimal readable loop

### Permissions and governance

- `claw-code`: central policy engine, approval tokens, hook-owned interception,
  recovery recipes
- `opencode`: mode-aware permissions and explicit approval UI state machine
- `OpenHarness`: permission modes, path rules, team permission sync
- `OpenClaw`: permission relay between native harness and control plane

### Session continuity and state

- `opencode`: durable prompt admission, safe promotion, session drain
- `claw-code`: per-workspace session namespaces and safe latest-session rules
- `OpenHarness`: snapshots, session-key lookup, transcript export
- `ohmo`: one runtime bundle per session key and bundle reuse by cwd
- `OpenClaw`: gateway-owned session binding and transcript projection
- `hermes-agent`: `resume_pending` markers after restart interruption

### Memory design

- `OpenHarness`: typed `MEMORY.md` index plus scoped memory files and freshness
  policy
- `ohmo`: personal memory overlay separate from project memory
- `hermes-agent`: layered external memory subsystem with tiered retrieval
- `nanobot`: lightweight file-backed continuity

### Delegation and team topology

- `opencode`: owner plus one narrow helper surface
- `OpenClaw`: explicit subagent spawn contract, context inheritance, sandbox
  and timeout posture
- `OpenHarness`: team lifecycle files, permission sync, background agent tasks
- `claw-code`: task registry, heartbeat, lane freshness board
- `learn-claude-code`: staged path from no delegation to selective helpers

### Gateway, channels, and event reinjection

- `OpenClaw`: gateway as control plane, media/session/history/abort methods,
  event reinjection
- `ohmo`: session-aware runtime pool for channel chats
- `OpenHarness`: channel adapters and always-on runtime overlay
- `hermes-agent`: long-lived gateway daemon and restart handling

### Providerization and fallback

- `hermes-agent`: providerized memory, MCP, auxiliary tasks, and profile-based
  fallback chain
- `OpenClaw`: provider/model fallback candidates with diagnostics and cooldowns
- `claw-code`: provider fallback config inside runtime governance

### Plugins, skills, and extension supply chain

- `OpenHarness`: plugin manifests loading skills, commands, hooks, MCP, and
  agents from trusted roots
- `OpenClaw`: provenance-aware plugin loader and capability registry
- `hermes-agent`: guarded skills hub and safe remote bundle install

## Branch Activation Heuristics

### Open the coding branch first when

- the job is code change plus verification
- the first unresolved risk is mode split, permission posture, or owner/helper
  boundary

### Open the always-on branch first when

- channel routing or session reinjection is the first hard problem
- the system must continue acting between direct user turns

### Open the memory branch only when at least one pressure is real

- cross-turn stable facts
- cross-session continuity
- shared team memory
- user personalization

### Open the governance branch when

- high-risk tools exist
- approval posture or auditability is a first-order concern

### Open the providerized branch when

- there is concrete backend replacement pressure
- one core must serve multiple surfaces
- fallback is part of correctness or uptime

### Open the lightweight branch when

- the bigger risk is overbuilding
- the current design is importing heavy patterns with no active pressure

## Shared Lessons

1. Start from job, user, action radius, and success/failure signal.
2. State the family read early and say which heavier family is not justified.
3. Borrow by subsystem pressure, not by brand imitation.
4. Ask the next highest-risk branch only; do not force fixed coverage.
5. Name the mechanisms being deferred and the trigger that would justify them
   later.
