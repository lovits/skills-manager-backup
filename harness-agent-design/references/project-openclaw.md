# OpenClaw Notes

## Why This Project Matters

OpenClaw is a strong source for always-on harnesses where the gateway becomes
the control plane. It shows how to keep one session identity alive across
channels, attachments, plugin surfaces, delegated runs, fallback chains, and
native hook relays.

Its codegraph clusters around:

- `src/agents/sessions/agent-session.ts`
- `src/gateway/server-methods/chat.ts`
- `src/plugins/loader.ts`
- `src/agents/subagent-spawn.ts`
- `src/agents/model-fallback.ts`
- `src/agents/harness/native-hook-relay.ts`

## Subsystem Methods

### Shared session abstraction

Borrow:

- one `AgentSession` abstraction reused by interactive, print, and RPC modes
- event subscription plus automatic persistence
- compaction, session switching, branching, and model/thinking controls behind
  one shared session facade

Use when:

- the same runtime core must serve multiple user surfaces

### Gateway as control plane

Borrow:

- gateway chat methods that own send, history, abort, inject, metadata, media,
  and in-flight run tracking
- session key normalization and binding services
- transcript readers and display projection separated from execution
- external event reinjection into the same session rather than spawning
  unrelated sessions

Use when:

- messages arrive from multiple channels or clients
- operators need one place to inspect and steer the runtime

Avoid when:

- the system is just one local foreground loop

### Plugin and capability registry

Borrow:

- plugin loader that discovers, validates, records provenance, and restores
  runtime registrations
- plugin-owned commands, hooks, embeddings, compaction providers, channel
  plugins, and dreaming sidecars
- activation sources and owner-trust checks before runtime enablement

Use when:

- capability sprawl is likely unless the control plane governs it

### Delegation boundary

Borrow:

- explicit subagent spawn parameters: task, model, sandbox, timeout, context,
  cleanup, and attachment handling
- session-store updates for child runs
- clear difference between control gateway actions and agent child actions
- spawn depth and child-count limits

Use when:

- child runtimes are real architecture, not just a convenience wrapper

### Fallback as runtime policy

Borrow:

- ordered fallback candidates across providers and models
- per-attempt attribution, skip reasons, cooldown handling, and final fallback
  summary error
- distinction between provider failure and runtime coordination failure

Use when:

- uptime depends on moving through a credible fallback chain

### Native hook relay and external control bridge

Borrow:

- relay registration with TTL, event allowlist, and per-session attribution
- explicit events such as `pre_tool_use`, `post_tool_use`,
  `permission_request`, and `before_agent_finalize`
- bridge path for external native harnesses to feed approvals and lifecycle
  signals back into the gateway-owned runtime

Use when:

- external runtimes or native clients must still participate in one control
  plane

## Question Angles This Project Justifies

- Is there real gateway pressure, or would that only add ceremony?
- Which events must re-enter the same session identity?
- Which capabilities belong in plugins versus the core runtime?
- What exactly may a subagent own, and what must remain gateway-owned?
- Is fallback just a convenience, or part of the correctness story?

## Design Warnings

- A gateway is expensive; use it only when lifecycle complexity is real.
- Plugin registries and delegated child runs need hard boundaries or they turn
  into uncontrolled runtime sprawl.
