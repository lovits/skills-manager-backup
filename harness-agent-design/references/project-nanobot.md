# Nanobot Notes

GitHub: [HKUDS/nanobot](https://github.com/HKUDS/nanobot)

## Why This Project Matters

nanobot is a strong method source for lightweight harnesses that still have
real runtime structure. It is useful when the right answer is not "keep
everything in one file forever," but also not "import a whole control plane."

The core lesson is not merely "stay small." It is:

- keep the owner loop readable
- split orchestration from execution
- split context assembly from execution
- treat tools, channels, providers, and subagents as adapters
- add durability and always-on features without losing legibility

Read nanobot as a disciplined small-runtime source, not as a toy or a stripped
clone of bigger harnesses.

Its codegraph and architecture docs cluster around:

- `nanobot/agent/loop.py`
- `nanobot/agent/runner.py`
- `nanobot/agent/context.py`
- `nanobot/agent/subagent.py`
- `nanobot/agent/tools/loader.py`
- `nanobot/channels/manager.py`
- `nanobot/providers/factory.py`
- `nanobot/session/manager.py`
- `nanobot/cron/service.py`
- `docs/nanobot-architecture-and-harness.md`

Read nanobot for subsystem questions like:

- what is the smallest runtime split that keeps the loop understandable
- when a small harness should separate loop, runner, and context
- how always-on channels can exist without requiring a heavy control plane
- how lightweight durability and memory should stay file-backed and legible
- how subagents should become isolated mini-runtimes instead of shared-state
  helpers

## Subsystem Methods

### Small readable owner loop

Borrow:

- one owner `AgentLoop` that still owns the whole turn lifecycle
- explicit turn states such as restore, compact, command, build, run, save,
  respond, done
- state-transition thinking instead of letting one giant function accumulate
  every concern
- a loop that stays easy to reason about even after adding commands,
  compaction, session restore, and response delivery

Use when:

- the first product goal is a clear, working assistant loop
- maintainability depends on a human being able to read the loop top to bottom
- the harness may grow, but must not become opaque

Avoid when:

- the system already has strong pressure for a separate control plane or
  distributed runtime

Deep lesson:

- "minimal" does not mean "undifferentiated"
- nanobot keeps the loop small by giving adjacent concerns their own seams

### Orchestrator versus execution core

Borrow:

- `AgentLoop` as product orchestration
- `AgentRunner` as shared LLM + tool-calling execution kernel
- `AgentRunSpec` style explicit run contract instead of hidden global state
- separation between session/channel/memory/command concerns and pure model +
  tool iteration

Use when:

- one execution kernel may be reused by CLI, API, subagents, or channels
- product behavior and execution behavior should evolve independently
- the user wants a harness that can stay small but still have clean boundaries

Avoid when:

- the whole runtime really is one trivial request-response call

One of nanobot's most reusable methods:

- orchestrate in one place
- execute in another

### Context assembly as a dedicated subsystem

Borrow:

- `ContextBuilder` as the single prompt assembly surface
- explicit layering of identity, workspace context, memory, skills, history,
  runtime metadata, and summaries
- runtime metadata treated as metadata, not mixed casually into the main prompt
- cache-aware and stability-aware prompt construction

Use when:

- prompt construction is becoming complex enough to deserve one owner
- context assembly risks leaking across loop/tool/channel code
- the harness needs clear reasoning about what the model actually sees

Avoid when:

- there is no real distinction between system prompt, history, and runtime
  context

### Adapterized tools, channels, and providers

Borrow:

- tools loaded through a registry/loader rather than hardcoded into the loop
- channel abstraction where each platform implements a common send/start/stop
  contract
- provider factory plus fallback wrapper rather than loop-owned provider
  branches
- extension by registration instead of extension by giant switch statements

Use when:

- the harness may need many tools or several chat surfaces
- providers or channels may vary without changing the owner loop
- you want extensibility without immediately jumping to plugin marketplaces

Avoid when:

- extension seams are solving imaginary future problems

Deep lesson:

- nanobot is extensible, but its extensibility still fits inside a readable
  local runtime

### Lightweight durability and session continuity

Borrow:

- session manager as a lightweight continuity layer
- file-backed artifacts and transcript/history persistence
- compact-then-save rather than infinite raw chat growth
- long-session support without requiring a service-grade session control plane
- enough durable state to survive restart and keep progress coherent

Use when:

- the assistant must resume work later but does not need heavyweight session
  routing
- simple persistence is enough for the product
- continuity matters more than platform-scale operability

Avoid when:

- a full remote daemon with complex session ownership is already justified

### Auto-compact and bounded history

Borrow:

- auto-compaction as a normal part of the runtime
- explicit history pressure management before the system becomes unusable
- bounded tool-result/history handling in a small harness
- durable transcript handoff alongside compaction

Use when:

- the harness needs long sessions but should stay lightweight
- the product would otherwise drown in raw history

Avoid when:

- conversations are always short-lived

### Subagent as an isolated mini-runtime

Borrow:

- subagent as a reduced but separate runtime, not a shared-state helper call
- distinct tool registry, tool context, file-state tracking, and workspace
  scope for child runs
- tighter child permissions and narrower capability sets
- child isolation as the answer to context pollution

Use when:

- one helper should isolate noisy search or side work
- a child needs constrained tools or a constrained workspace
- the parent must stay clean and legible

Avoid when:

- "subagent" is being used to hide an unclear main loop

Deep lesson:

- nanobot makes subagents smaller runtimes
- not invisible background magic

### Always-on without over-platformization

Borrow:

- channel manager that can host many messaging surfaces while keeping one core
  loop
- always-on capability added as an overlay instead of a full control-plane
  redesign
- cron/service and channel/runtime edges that remain local and inspectable

Use when:

- the assistant should live in chats or simple background contexts
- remote surfaces are needed, but not a full gateway architecture

Avoid when:

- routing, pairing, multi-client control, or restart-resume complexity already
  demands a heavier runtime

## Question Angles This Project Justifies

- What is the smallest split that keeps this harness readable: one loop only,
  or loop + runner + context?
- Is this really heavy enough to need a control plane, or would a disciplined
  local runtime do?
- Which state can stay file-backed and simple?
- Does subagent pressure mean isolated mini-runtimes, or just one better owner
  loop?
- Are channels just overlays on one runtime, or do they introduce real routing
  pressure?

## Design Warnings

- Do not treat nanobot as permission to keep everything in one blob forever.
- Do not import heavyweight provider, gateway, or team abstractions before
  actual pressure appears.
- Do not add subagents if the real problem is still an unclear parent loop.
- Do not confuse "supports many channels" with "needs a full control plane."
