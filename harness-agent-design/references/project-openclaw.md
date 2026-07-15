# OpenClaw Notes

GitHub: [openclaw/openclaw](https://github.com/openclaw/openclaw)

## Why This Project Matters

OpenClaw is a strong source for always-on harnesses where the gateway becomes
the control plane. It shows how to keep one session identity alive across
channels, attachments, plugin surfaces, delegated runs, fallback chains, and
native hook relays.

Do not reduce OpenClaw to "multi-channel bot" or "gateway app." Its deeper
lesson is a long-lived control plane that owns:

- ingress from many channels and clients
- deterministic routing to one agent/session owner
- session-key discipline and reset policy
- operator-facing inspection and steering surfaces
- plugin and bundle capability loading with provenance
- channel security posture such as pairing, allowlists, and mention gating
- delegated runs and external runtime bridges without surrendering ownership

This makes it a strong source for always-on harness design, not just messaging
integrations.

Its codegraph clusters around:

- `src/agents/sessions/agent-session.ts`
- `src/gateway/server-methods/chat.ts`
- `src/plugins/loader.ts`
- `src/agents/subagent-spawn.ts`
- `src/agents/model-fallback.ts`
- `src/agents/harness/native-hook-relay.ts`
- `docs/concepts/architecture.md`
- `docs/concepts/session.md`
- `docs/channels/channel-routing.md`
- `docs/concepts/multi-agent.md`
- `docs/gateway/security.md`
- `docs/tools/plugin.md`

Read OpenClaw for subsystem questions like:

- when a gateway is truly justified
- how reply ownership should stay deterministic
- how session keys should be shaped across DM, groups, channels, threads, and
  hooks
- how one shared session abstraction can serve several run modes
- what durable session state belongs to the control plane instead of the model
- what belongs to the gateway versus to the agent runtime
- how plugins, bundles, and external runtimes stay governable
- how multi-agent routing differs from planner-tree delegation
- how security defaults shape channel-facing harnesses
- how operator controls, pairing, and inspection belong to runtime design

## Subsystem Methods

### Shared session abstraction

Borrow:

- one `AgentSession` abstraction reused by interactive, print, and RPC modes
- event subscription plus automatic persistence
- compaction, session switching, branching, and model/thinking controls behind
  one shared session facade
- one execution/session surface that also owns bash execution and context
  controls instead of letting each client invent its own loop wrapper
- a shared execution/session surface that different clients can inspect and
  steer without inventing separate runtimes
- separation between transcript storage, display projection, and execution
  control
- the idea that the same session may be surfaced via CLI, web UI, native app,
  and channel bridge without becoming different logical conversations

Use when:

- the same runtime core must serve multiple user surfaces
- the user wants one conversation identity across operator surfaces
- the harness needs session controls such as reset, compaction, branch, model,
  or think-level toggles that should not depend on one UI

Deep lesson:

- OpenClaw keeps "run mode" separate from "session ownership"
- the same logical conversation can survive several presentation modes without
  fragmenting the runtime

### Gateway as control plane

Borrow:

- gateway chat methods that own send, history, abort, inject, metadata, media,
  and in-flight run tracking
- session key normalization and binding services
- transcript readers and display projection separated from execution
- external event reinjection into the same session rather than spawning
  unrelated sessions
- one long-lived process that owns provider/channel connections and routing
- deterministic reply routing where the model does not choose the channel
- typed client/server protocol for operators, nodes, web UIs, and automation
- gateway-owned operations snapshot, health, and supervision model
- the distinction that the gateway is the control plane and the assistant is
  the product running through it

Use when:

- messages arrive from multiple channels or clients
- operators need one place to inspect and steer the runtime
- one host process should own inboxes, presence, tools, nodes, and control UIs
- external clients must steer the same runtime instead of running parallel ones

Avoid when:

- the system is just one local foreground loop

Deep lesson:

- OpenClaw does not make the gateway equal to "just transport"
- it makes the gateway the authority for routing, session ownership, and
  operability

### Plugin and capability registry

Borrow:

- plugin loader that discovers, validates, records provenance, and restores
  runtime registrations
- plugin-owned commands, hooks, embeddings, compaction providers, channel
  plugins, and dreaming sidecars
- activation sources and owner-trust checks before runtime enablement
- cold manifest inspection before runtime code is loaded
- activation planning by capability such as provider, channel, tool, and hook
- setup/auth evidence that can be checked before full plugin activation
- allow/deny policy, slots, and runtime inspect surfaces
- distinction between native runtime plugins and compatible bundle-style
  plugins
- source-aware installs from local path, git, npm, or marketplace
- the idea that capabilities should be inspectable as cold manifests and as
  live runtime registrations
- operator install policy before code or bundle activation

Use when:

- capability sprawl is likely unless the control plane governs it
- the harness must grow via plugins without losing trust boundaries
- skills, channels, tools, or provider surfaces may come from different supply
  paths but still need one governance model

Reusable OpenClaw method: treat plugin growth as a runtime-governed supply
chain problem, not informal folder loading.

### State-rich session contract

Borrow:

- durable session metadata that stores more than transcript location
- explicit session state for compaction checkpoints, context budget route,
  plugin next-turn injections, quota suspension, and child-lane recovery
- lane execution states such as active, draining, suspended, resuming, and
  circuit-open
- the idea that recovery and steering artifacts should live in session state,
  not in hidden process memory

Use when:

- the runtime needs operators to understand why a session is paused, wedged, or
  resumable
- more than one subsystem can affect whether a session may continue
- plugin or subagent behavior can inject future-turn control data

Avoid when:

- the runtime is short-lived and restart-from-scratch is always acceptable

### Deterministic reply ownership and channel routing

Borrow:

- reply routing controlled by host configuration rather than model choice
- explicit channel/account/peer/guild/team binding rules
- routing priority order and tie-breaking documented as runtime behavior
- session keys shaped by source kind: DM, group, channel, thread, topic,
  webhook, cron
- guarded inbound recording so a message observation does not always create a
  durable conversation
- special handling for shared-main DM modes so route ownership is not silently
  stolen

Use when:

- the agent lives in several inboxes, chats, or rooms
- the same user may reach the system through different surfaces
- reply routing mistakes would create trust or context leakage problems

Deep lesson:

- OpenClaw treats routing as a host decision
- not as a model inference problem

### Session scope, continuity, and reset policy

Borrow:

- configurable DM scope from shared main session to per-channel-peer isolation
- identity linking across channels when the same person should share context
- explicit daily reset, idle reset, manual reset, and queue discard rules
- session lifecycle timestamps separated by purpose: started, interaction, and
  bookkeeping updates
- freshness rules that distinguish true interaction from background churn
- maintenance mode and pruning policy that preserve durable external pointers

Use when:

- a personal assistant must be continuous but not endlessly sticky
- multi-user DM leakage is a real risk
- session freshness and continuity need explicit policy instead of accidental
  chat-log growth

This is a major method source because it helps ask:

- who should share one session
- what creates a new session
- what background activity should not extend freshness
- what resets are product behavior versus maintenance behavior

### Delegation boundary

Borrow:

- explicit subagent spawn parameters: task, model, sandbox, timeout, context,
  cleanup, and attachment handling
- session-store updates for child runs
- clear difference between control gateway actions and agent child actions
- spawn depth and child-count limits
- delegation treated as bounded child runtime ownership, not vague helper calls
- reintegration rules that preserve parent ownership of the main session
- child lifecycle visibility in the same control-plane environment
- a distinction between multi-agent routing across personas and subagent
  delegation inside one persona

Use when:

- child runtimes are real architecture, not just a convenience wrapper
- delegated work can carry attachments, run in sandboxes, or outlive one turn
- the user is tempted to say "multi-agent" when they may actually need only
  routing or isolated personas

OpenClaw is especially useful here because it separates two often-confused
patterns:

- many isolated agents bound to channels/accounts
- one agent spawning bounded child runs

### Fallback as runtime policy

Borrow:

- ordered fallback candidates across providers and models
- per-attempt attribution, skip reasons, cooldown handling, and final fallback
  summary error
- distinction between provider failure and runtime coordination failure
- auth-profile rotation and model failover as operator-configured runtime
  behavior
- visible fallback diagnostics so outages and skips are legible
- fallback as part of uptime and correctness, not just "try another model"

Use when:

- uptime depends on moving through a credible fallback chain
- different channels or surfaces may depend on different provider behavior
- the user needs to know whether failure is model-side, provider-side, or
  control-plane-side

### Native hook relay and external control bridge

Borrow:

- relay registration with TTL, event allowlist, and per-session attribution
- explicit events such as `pre_tool_use`, `post_tool_use`,
  `permission_request`, and `before_agent_finalize`
- bridge path for external native harnesses to feed approvals and lifecycle
  signals back into the gateway-owned runtime
- approval throttling windows and bounded relay participation
- explicit control-bridge semantics rather than opaque callback glue
- the idea that native apps, ACP clients, or external runtimes can participate
  in one governed session without taking over ownership
- event relays as bounded, attributed, expiring participation rights

Use when:

- external runtimes or native clients must still participate in one control
  plane
- approvals, tool events, or lifecycle events may originate outside the agent
  loop but still need to affect the same run

### Security-first channel harness defaults

Borrow:

- pairing as default DM posture for unknown senders
- fail-closed group/channel allowlist defaults
- mention gating and visible-reply modes for shared rooms
- separation between ambient room events and visible agent replies
- sandbox policy that treats non-main sessions differently from trusted main
  sessions
- device pairing and identity pinning for control-plane and node connections
- idempotency requirements for side-effecting gateway methods

Use when:

- the assistant is exposed on real messaging or remote control surfaces
- multi-user or semi-public contexts exist
- "always-on" should not imply "always-responding"

This is a crucial lesson from OpenClaw: always-on harnesses need access and
reply policy as first-class runtime design, not as an afterthought.

### Multi-surface operator and node model

Borrow:

- operators, web UIs, native apps, automations, and nodes all talking to the
  same gateway protocol
- node role separation from ordinary control-plane clients
- one protocol with discovery metadata, handshake, auth, pairing, and server
  push events
- long-lived daemon plus supervised lifecycle as the base operating model

Use when:

- the harness may have devices, apps, and automations around one assistant
- the runtime should be inspectable or steerable from more than one surface

### Agent/persona routing rather than hierarchy-by-default

Borrow:

- agents as isolated personas with their own workspace, auth, and sessions
- deterministic bindings from channels/accounts/peers to one agent
- cross-agent sharing only when explicitly configured
- avoidance of manager-of-managers hierarchy as the default architecture

Use when:

- the user really wants many separate "brains" on one gateway host
- multiple people or personas should coexist without context bleed
- the design pressure is routing isolation, not nested planning

## Question Angles This Project Justifies

- Is there real gateway pressure, or would that only add ceremony?
- Does one shared session abstraction need to serve more than one run mode?
- Which events must re-enter the same session identity?
- Which runtime states must be durable and operator-visible even when the model
  is idle?
- Which capabilities belong in plugins versus the core runtime?
- What exactly may a subagent own, and what must remain gateway-owned?
- Is fallback just a convenience, or part of the correctness story?
- Is reply routing deterministic and host-owned, or is the model being asked to
  improvise channel choice?
- Which chats share one session, and which must be isolated?
- Which background events should count as real freshness, and which should not?
- Is the design really multi-agent, or is it multi-persona routing?
- Which surfaces are operators, which are nodes, and which are end-user
  channels?
- Should ambient room context be visible reply-by-default or message-tool-gated?
- Which plugin or bundle sources are trusted enough to load at runtime?

## Design Warnings

- A gateway is expensive; use it only when lifecycle complexity is real.
- Plugin registries and delegated child runs need hard boundaries or they turn
  into uncontrolled runtime sprawl.
- Do not copy OpenClaw just because the user said "assistant" or "chatbot."
- Do not let the model choose transport, session owner, or visible reply path
  when the host can decide deterministically.
- Do not confuse channel richness with agent intelligence; much of OpenClaw's
  value is harness ownership and runtime policy.

## Summary Read

Use OpenClaw deeply when the design pressure sounds like:

- always-on assistant
- multi-channel ingress
- one control plane across apps, channels, and nodes
- session routing and reply ownership matter
- plugin growth and runtime security matter
- operator visibility and steering matter

Use it lightly when the design pressure sounds like:

- one local user
- one foreground loop
- little need for deterministic routing or remote control
- no real plugin or channel governance burden
