# Hermes Agent Notes

GitHub: [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent)

## Why This Project Matters

hermes-agent is a major method source for harnesses that are not just "an
agent loop with many tools," but a runtime assembly made of replaceable,
long-lived subsystems:

- one shared agent core reused by CLI, gateway, cron, and desktop-adjacent
  surfaces
- providerized main model, auxiliary tasks, memory backend, MCP servers, and
  delivery surfaces
- explicit session lifecycle and restart-resume state
- first-class background automation, not just foreground chat
- governed capability growth through skills, MCP, and plugin-style providers

For this skill, Hermes should not be reduced to "multi-provider" or "it has
memory." The deeper lesson is that Hermes treats the harness as an operational
runtime with several distinct subsystems that can evolve independently while
still sharing one core conversation engine.

Its codegraph strongly clusters around:

- `run_agent.py`
- `agent/auxiliary_client.py`
- `agent/context_compressor.py`
- `agent/memory_manager.py`
- `agent/memory_provider.py`
- `agent/tool_guardrails.py`
- `gateway/run.py`
- `gateway/session.py`
- `tools/mcp_tool.py`
- `tools/skills_hub.py`
- `cron/scheduler.py`
- `docs/session-lifecycle.md`

Read Hermes as a method source for at least these subsystem questions:

- when one harness core should really serve CLI, gateway, cron, and remote
  surfaces
- what should be providerized: only the main model, or also auxiliary tasks,
  memory, MCP, and delivery/runtime services
- how session identity, reset, suspension, and restart-resume should be
  modeled explicitly
- how context compression and memory should behave as managed subsystems,
  rather than as ad hoc prompt hacks
- when MCP should be treated as long-lived runtime infrastructure
- how skills and remote capability installs stay governable
- how unattended automation changes tool governance and prompt safety
- how fallback chains become uptime/correctness policy rather than hidden glue

## Subsystem Methods

### Shared core across surfaces

Borrow:

- one `AIAgent` core reused across CLI, gateway, and cron execution paths
- surface-specific session source and working-directory rules around a shared
  engine
- a harness architecture where messaging adapters, cron delivery, and terminal
  interaction are overlays on the same core runtime instead of separate agents
- one source of truth for conversation loop, tool execution, persistence, and
  context handling

Use when:

- one product must exist in foreground and remote forms
- the user expects one coherent assistant behavior across surfaces
- runtime reuse pressure is semantic, not just branding

Avoid when:

- the remote surface behaves like a different product with different state and
  safety semantics

Deep lesson:

- Hermes shares the core only where the semantics truly match
- it does not equate "same model" with "same harness"

### Session lifecycle as a first-class runtime subsystem

Borrow:

- explicit `SessionSource`, `SessionEntry`, and `SessionStore` style modeling
- deterministic session keys shaped by platform, chat kind, thread, and user
  isolation rules
- separate lifecycle flags for `suspended`, `resume_pending`, fresh reset,
  expiry-finalized, and auto-reset reasons
- priority-ordered session recovery logic instead of inferring state from
  transcript tail shape
- restart-resume behavior driven by metadata, not by vague "load the latest
  chat"
- explicit transcript store plus session metadata store split
- session continuity policy as product behavior: idle reset, daily reset,
  manual reset, crash recovery, pruning

Use when:

- the harness may live across restarts, channel reconnects, or long idle gaps
- one session may be resumed, reset, or suspended for different reasons
- who shares a conversation lane is a real architecture question

Avoid when:

- the whole system is a short-lived local loop with no durable session
  semantics

This is one of Hermes' strongest reusable methods. It helps the skill ask:

- what exactly identifies a conversation lane
- what survives restart versus what forces a reset
- what crash marker preserves intent without pretending the turn completed

### Gateway daemon and remote delivery runtime

Borrow:

- gateway as a long-lived daemon that owns adapters, agent cache, event drain,
  status delivery, and restart handling
- routing and reply delivery treated as runtime concerns, not left to model
  improvisation
- per-session cached agents with TTL/cap eviction for always-on deployments
- delivery and status messaging that can be sanitized differently from the core
  assistant response
- remote-surface operability: progress updates, queue handling, notification
  hygiene, and adapter lifecycle management

Use when:

- the assistant must live outside a single terminal
- messaging uptime and routing correctness are first-order concerns
- operators need the runtime, not just the model, to stay alive under noisy
  network conditions

Avoid when:

- there is no daemon, no inbox, and no long-lived adapter process

Deep lesson:

- Hermes pushes "remote chat agent" design toward service-runtime design
- that is lighter than a full control plane like OpenClaw, but much richer than
  a local chat loop

### Providerized execution and fallback policy

Borrow:

- providerization not only for the main model, but for model-specific timeout,
  stale-timeout, and auth behavior
- ordered fallback chains that distinguish temporary retry, provider rotation,
  and real exhaustion
- profile-scoped config and credential resolution rather than one global
  provider switch
- provider/model compatibility checks as runtime logic
- fallback traces buffered and surfaced only when the chain really fails
- different provider choices for main turns versus auxiliary tasks

Use when:

- fallback is part of uptime or correctness
- users may run through aggregators, direct APIs, OAuth-backed routes, or
  custom endpoints
- backend behavior differs enough that timeouts, auth, and stale-connection
  policy must be explicit

Avoid when:

- there is one stable backend and no real replacement pressure

Deep lesson:

- Hermes providerization is operational
- it exists to survive the real backend mess, not to look architecturally pure

### Auxiliary side-task router

Borrow:

- one shared auxiliary router for compression, vision, extraction, search-like
  side work, and other secondary LLM tasks
- explicit resolution chains for text and vision side tasks
- per-task provider/model override without duplicating all main-loop provider
  logic
- interrupt protection for atomic auxiliary work such as context compression
- a design where side tasks can fall back independently of the main response
  model

Use when:

- the harness has several secondary LLM tasks beyond the main answer loop
- side tasks should share one policy and fallback system
- context compression or similar work must not be interrupted casually

Avoid when:

- there are no meaningful auxiliary LLM tasks

This is a major Hermes contribution because it helps the skill ask:

- are side tasks first-class runtime behavior
- do they need their own fallback, latency, and interruption rules

### Context compression as managed runtime state

Borrow:

- context compression as an explicit subsystem with its own engine and
  lifecycle hooks
- summary handoff language designed to stop stale-task resumption
- session transition hooks for end, reset, start, and carry-over behavior
- deterministic pre-pruning of tool output before LLM summarization
- auxiliary-model compression with failure cooldowns and fallback behavior
- compression-aware metadata so frontends can distinguish summary artifacts
  from real conversation turns

Use when:

- long sessions must compact safely without mutating the harness story
- the biggest risk is stale work or old summaries hijacking new turns
- context carry-over must be governed explicitly

Avoid when:

- the session will stay short and compaction never really triggers

Deep lesson:

- Hermes treats compression as a session-state transition problem
- not as "ask a model to summarize and paste the result"

### Layered memory subsystem with provider contract

Borrow:

- one `MemoryManager` as the integration point for pluggable providers
- a clear `MemoryProvider` contract: initialize, prompt block, prefetch,
  sync-turn, tool schemas, session hooks, pre-compress hook, delegation
  observation, backup paths
- one-external-provider limit to prevent schema sprawl and conflicting memory
  backends
- prefetch-before-turn plus sync-after-turn as standard memory choreography
- fenced or scrubbed recalled-context injection so memory payloads do not leak
  into visible assistant text
- optional memory tools exposed only when toolset policy allows them
- memory hooks that understand resets, session switches, compression, and
  subagent boundaries

Use when:

- memory is a real subsystem with retrieval and writeback behavior
- different backends may implement memory differently but must honor one host
  contract
- stale memory leakage into the UI or into wrong sessions is a real risk

Avoid when:

- memory is just one local markdown file and no backend variation exists

Deep lesson:

- Hermes memory design is not "more memory"
- it is host-owned orchestration of recall, writeback, tool exposure, and
  lifecycle boundaries

### MCP as long-lived runtime infrastructure

Borrow:

- dedicated background event loop and daemon thread for long-lived MCP
  connections
- per-server transport choice across stdio, streamable HTTP, and SSE
- connection keepalive, reconnection, and timeout policy as host concerns
- environment filtering and stderr redirection so MCP servers do not corrupt
  the UI/runtime
- optional server-initiated sampling and parallel-tool-call capability
- concurrency model where MCP tool execution is scheduled safely onto the
  persistent loop

Use when:

- MCP is part of runtime infrastructure rather than a thin one-shot wrapper
- server liveness, reconnect behavior, or transport choice matters
- one harness may carry several persistent MCP integrations

Avoid when:

- "MCP" just means one simple local connector with no runtime lifecycle

Deep lesson:

- Hermes helps the skill separate "tool calling through MCP" from "operating an
  MCP runtime"

### Skills hub and capability supply chain

Borrow:

- guarded remote skill fetch/install pipeline with trust levels and provenance
- quarantine, audit log, taps, and lock-file state for installed capabilities
- path normalization, redirect limits, SSRF-safe fetch, and trusted-repo policy
- skill source adapters instead of one hardcoded marketplace
- distinction between source metadata, downloaded bundle, quarantine, and live
  install location

Use when:

- the harness must acquire capabilities from outside the local repo
- skills are a governed extension surface, not just copied folders
- provenance and uninstall safety matter

Avoid when:

- the whole skill surface is local and manually curated

This is especially valuable for the skill because it pushes questions like:

- who is allowed to add capabilities
- how remote installs become auditable
- whether skills are a runtime supply-chain problem

### Unattended cron and background automation

Borrow:

- cron as a first-class execution surface, not just an external wrapper script
- toolset narrowing for unattended runs
- prompt-injection scanning on the fully assembled cron prompt, including
  loaded skill content and injected context
- delivery-target resolution as host policy
- distinct timeouts, inactivity limits, and output-delivery behavior for
  non-interactive runs
- support for MCP, scripts, and skills inside scheduled jobs with extra
  safeguards
- provider fallback and session persistence for unattended work

Use when:

- the assistant must continue acting without a live foreground user
- scheduled jobs, recurring checks, or delayed delivery are part of the
  product
- automation safety differs from interactive safety

Avoid when:

- background work is hypothetical and no unattended execution exists

Deep lesson:

- Hermes treats cron as a separate harness pressure with its own governance,
  prompt safety, and delivery semantics

### Tool-loop and runtime guardrails

Borrow:

- side-effect-free tool loop guardrail primitives that separate decision from
  runtime enforcement
- different treatment for idempotent versus mutating tools
- thresholds for exact-repeat failure, same-tool failure, and no-progress loops
- warnings by default with optional hard-stop posture
- host-level classification of tool progress/failure instead of trusting the
  model to self-regulate

Use when:

- long-running sessions can get stuck in tool loops
- some tools mutate state and require stronger loop governance
- the runtime needs inspectable policy rather than vague prompt nudges

Avoid when:

- there are no long-running tool loops or side-effecting operations

## Question Angles This Project Justifies

- Is this really one shared core across several surfaces, or several products
  with a shared library?
- What exactly is providerized here: main model, auxiliary tasks, memory, MCP,
  cron runtime, or delivery surface?
- Does restart continuity need a true session lifecycle model, or only replay
  of the latest transcript?
- Is memory just a note store, or a host-managed provider contract with hooks?
- Is MCP just another tool adapter, or a persistent runtime subsystem?
- Will unattended work exist, and if it does, how do its tool and prompt
  policies differ from interactive mode?
- Should skills be copied manually, or treated as an auditable capability
  supply chain?
- Which fallback chain is part of correctness, and which is only best effort?

## Design Warnings

- Do not copy Hermes' providerization if backend replacement pressure is still
  hypothetical.
- Do not import its gateway/cron machinery into a foreground-only assistant.
- Do not treat memory as "just retrieval" if your real problem is session
  boundaries and stale-state control.
- Do not add MCP infrastructure if you only need a simple local tool wrapper.
- Do not ignore unattended-mode safety if cron or background execution is real;
  interactive governance is not enough.
