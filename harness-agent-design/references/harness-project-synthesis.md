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
- `/Users/qianye/Downloads/找工作/DL/tradingagents`
- `/Users/qianye/Downloads/找工作/DL/learn-claude-code`

## Family Detection Map

### Coding harness

Signals:

- edits code, runs shell, verifies results
- approval posture matters
- plan/build split or owner/helper split may matter

Lean first on:

- `opencode` for mode split, durable prompt admission, context-epoch
  discipline, permission state machine, queue semantics, and child-session
  inspectability
- `claw-code` for central governance, policy, recovery, and task freshness
- `learn-claude-code` for mechanism-first escalation from simple loop upward

### Always-on or gateway harness

Signals:

- external events, multiple channels, background work
- restart/resume and session reinjection matter
- operator inspectability matters

Lean first on:

- `OpenClaw` for gateway control plane, deterministic reply routing,
  session-key discipline, delegated runs, native hook relay, plugin supply
  governance, pairing/allowlist security posture, and fallback chains
- `OpenHarness` plus `ohmo` for reusable core plus channel-facing overlay,
  session-key runtime reuse, and snapshot restore
- `hermes-agent` when restart-resume and remote daemon behavior are central

### Providerized shared-core harness

Signals:

- more than one backend is truly in play
- memory, MCP, auxiliary work, or surfaces may swap providers
- one shared core should serve CLI, remote surfaces, or unattended execution

Lean first on:

- `hermes-agent` for providerized subsystems, auxiliary fallback routing,
  session lifecycle, and unattended runtime pressure
- `TradingAgents` when provider/model capability quirks and structured-output
  compatibility are central, especially when stages need typed outputs or
  separate reasoning tiers
- `OpenClaw` for runtime fallback chains across provider/model candidates
- `OpenHarness` when the shared core is an assembly platform, not just provider
  abstraction

### Role-specialized multi-agent pipeline

Signals:

- the domain naturally decomposes into stable specialist roles
- outputs should pass through analyst, debate, judge, or approval stages
- resumable staged analysis matters more than dynamic child spawning

Lean first on:

- `TradingAgents` for fixed role graph, specialist tool surfaces, debate/judge
  loops, quick/deep reasoning tiers, per-entity checkpoint isolation,
  structured-output contracts, and reflective decision memory
- `learn-claude-code` when the user first needs the distinction between helper
  runs, teammates, protocols, autonomy, and worktree isolation

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
  promotion, queue/drain semantics, active variant state, primary loop plus
  narrow inspectable helper
- `claw-code`: one owner loop with runtime-owned governance, stateful policy,
  bounded recovery, and lane-aware execution posture
- `TradingAgents`: staged specialist graph with role-specific tool surfaces,
  quick versus deep reasoning tiers, and judge/debate ownership rules
- `OpenClaw`: shared session abstraction across interactive, print, RPC,
  control-plane clients, and node-attached surfaces
- `hermes-agent`: one shared `AIAgent` core reused across CLI, gateway, and
  cron, plus a separate auxiliary side-task router
- `OpenHarness`: shared query engine plus control-surface overlays and manifest
  assembly, with pre-turn and post-turn memory lifecycle around the engine
- `nanobot`: minimal readable loop

### Permissions and governance

- `claw-code`: runtime-level config authority, central policy engine, approval
  state, hook-owned interception, side-effect-aware governance, bounded
  recovery recipes, and unified control over plugin/MCP/provider posture
- `opencode`: mode-aware permissions, explicit approval UI state machine,
  action-language metadata, and temporary allow posture
- `OpenHarness`: permission modes, path rules, team permission sync
- `OpenClaw`: permission relay between native harness and control plane,
  channel-facing access posture, pairing, mention gating, and visible-reply
  governance, plus plugin activation trust and native relay participation
- `hermes-agent`: tool loop guardrails, skills-hub trust/provenance checks,
  MCP env filtering/stderr isolation, and stricter unattended cron posture

### Session continuity and state

- `opencode`: durable prompt admission, safe promotion, session drain,
  exact retry semantics, process-local busy ownership, and context epoch
  refresh discipline
- `claw-code`: per-workspace session namespaces, workspace fingerprints, safe
  latest-session rules, and runtime-owned resume semantics
- `OpenHarness`: snapshots, session-key lookup, transcript export
- `ohmo`: one runtime bundle per session key and bundle reuse by cwd
- `OpenClaw`: gateway-owned session binding, transcript projection, DM-scope
  policy, session-key shaping by channel kind, reset policy, and route
  ownership rules, plus durable lane/subagent/plugin state in the session
  contract
- `hermes-agent`: explicit session-source/session-entry/session-store model,
  deterministic session keys, `resume_pending` versus `suspended`, idle/daily
  reset policy, cached-agent eviction, and crash-recovery markers
- `TradingAgents`: per-entity SQLite checkpoints and deterministic thread IDs
  for resumable staged runs

### Memory design

- `OpenHarness`: typed `MEMORY.md` index plus scoped memory files and freshness
  policy, signatures, truncation rules, plus session-memory and durable-memory
  extraction split
- `ohmo`: personal memory overlay separate from project memory
- `hermes-agent`: host-owned memory manager, provider contract, prefetch +
  sync choreography, fenced recalled context, and session/compression hooks
- `TradingAgents`: append-only decision log with pending decision capture,
  realized-outcome reflection, and cross-entity lessons
- `nanobot`: lightweight file-backed continuity

### Delegation and team topology

- `opencode`: owner plus one narrow helper surface, helper sessions as visible
  tabs/details rather than opaque background magic
- `TradingAgents`: fixed role graph, specialist stages, debate/judge loops, and
  graph-state handoff rather than dynamic spawn-first delegation, plus typed
  output contracts between stages
- `OpenClaw`: explicit subagent spawn contract, context inheritance, sandbox
  and timeout posture, plus separation between many routed personas and true
  child delegation
- `OpenHarness`: team lifecycle files, permission sync, background agent tasks
- `claw-code`: task registry, lifecycle states, heartbeat, freshness board,
  blocked/stale visibility, and policy-readable lane metadata
- `learn-claude-code`: staged path from no delegation to selective helpers

### Gateway, channels, and event reinjection

- `OpenClaw`: gateway as control plane, typed protocol, media/session/history/
  abort methods, event reinjection, deterministic reply routing, and multi-role
  clients/nodes, with freshness rules that distinguish interaction from
  background churn
- `ohmo`: session-aware runtime pool for channel chats
- `OpenHarness`: channel adapters and always-on runtime overlay
- `hermes-agent`: long-lived gateway daemon, adapter lifecycle, per-session
  agent cache, queued event drain, restart handling, and remote status/delivery
  hygiene

### Providerization and fallback

- `hermes-agent`: providerized memory, MCP, auxiliary tasks, profile-based
  fallback chain, per-task overrides, and provider/model timeout policy
- `TradingAgents`: declarative model capability table, lazy provider factory,
  and structured-output/runtime compatibility contract
- `OpenClaw`: provider/model fallback candidates with diagnostics, cooldowns,
  skip reasons, and final runtime attribution
- `claw-code`: provider fallback kept under runtime governance rather than
  hidden in backend glue

### Plugins, skills, and extension supply chain

- `OpenHarness`: plugin manifests loading skills, commands, hooks, MCP, and
  agents from trusted roots, plus app overlays such as `ohmo`
- `OpenClaw`: provenance-aware plugin loader, allow/deny policy, slots,
  cold-versus-runtime inspection, activation planning, setup/auth evidence, and
  compatible bundle/native plugin split
- `hermes-agent`: guarded skills hub, quarantine/audit/lock state, safe remote
  bundle install, and multi-source registry adapters

### Background automation and unattended work

- `hermes-agent`: cron as a first-class harness surface with toolset narrowing,
  assembled-prompt scanning, delivery-target policy, session persistence, and
  fallback-aware unattended runs
- `OpenHarness`: background task/team lifecycle, JSON-line worker framing,
  restart semantics, and durable task records when automation is part of the
  runtime
- `OpenClaw`: heartbeat, cron, queued event drain, and freshness rules for
  long-lived control-plane work
- `nanobot`: local cron/runtime edges without a full control-plane platform

### State, checkpoints, and recovery artifacts

- `OpenClaw`: durable session contract with compaction checkpoints, quota
  suspension, plugin next-turn injections, lane execution states, and child
  recovery markers
- `claw-code`: recovery recipes, workspace-safe session identity, and
  visibility of stale versus blocked lanes
- `opencode`: exact resume identity, process-local busy ownership, and safe
  promotion boundaries for queued prompts
- `TradingAgents`: per-entity SQLite checkpoints and deterministic thread IDs
- `OpenHarness`: session snapshots, file-backed session memory, and explicit
  extraction/compaction lifecycle
- `hermes-agent`: restart-resume states, cached-agent eviction, and
  session-store recovery markers

### Capability supply, hooks, MCP, and control surfaces

- `OpenClaw`: provenance-aware plugin loader, capability-based activation
  planning, bundle versus native plugin split, and native hook relays as
  bounded participants
- `OpenHarness`: manifest-loaded skills, commands, agents, hooks, MCP, and a
  unified runtime control surface for plan/worktree/task/team/cron state
- `claw-code`: hooks, plugins, MCP, and provider behavior kept under one
  runtime governance contract
- `hermes-agent`: guarded skills hub, plugin-style providers, and MCP as a
  long-lived runtime surface
- `learn-claude-code`: mechanism-order pressure test for when tools, hooks,
  skills, MCP, or plugins should even exist

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
- unattended execution, MCP runtime, or memory backend variation is real

### Open the state/recovery branch when

- checkpoint, resume, recovery, or freshness policy can invalidate trust
- a crash or interruption story matters to correctness, not just convenience

### Open the capability-supply branch when

- the architecture question is what should be loadable or activatable
- tools, MCP, skills, plugins, hooks, or commands are part of the runtime seam

### Open the background-automation branch when

- tasks, cron, workers, heartbeat, or unattended execution are real
- some execution continues after the foreground turn ends

### Open the multi-agent branch first when

- the job sounds like a stable specialist pipeline, debate graph, or explicit
  judge/approval chain
- the user says "multi-agent" but the real design choice may be fixed roles
  versus dynamic child runs

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
6. Read claw-code as a full governance-and-recovery method source, not just as
   a source of approvals or hooks.
7. Read opencode as a full staged-turn and durable-prompt method source, not
   just as a source of `plan/build`.
8. Read Hermes as a runtime-subsystem method source, not just as a source of
   "multi-provider" or "memory."
9. Read TradingAgents as a role-graph and reflective-memory source, not just as
   a source of "more personas."
10. Read nanobot as a disciplined small-runtime source, not just as a source of
    minimalism slogans.
11. Read learn-claude-code as a pressure-ordering method source, not just as a
    tutorial catalog.
12. Read OpenHarness as an assembly-platform source, not just as a source of
    plugins or `ohmo`.
13. Read OpenClaw as a control-plane and session-state method source, not just
    as a source of channels or gateway transport.
