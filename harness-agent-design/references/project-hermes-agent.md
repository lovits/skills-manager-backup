# Hermes Agent Notes

## Why This Project Matters

hermes-agent is a strong source for providerized harnesses and shared-core
deployments. It is useful when the design pressure is not one tool loop but a
stack of replaceable subsystems: gateway, main agent loop, memory backend,
skills hub, MCP, auxiliary provider routing, and restart-resume behavior.

Its codegraph clusters around:

- `gateway/run.py`
- `run_agent.py`
- `plugins/memory/openviking/__init__.py`
- `tools/skills_hub.py`
- `agent/auxiliary_client.py`
- `tools/mcp_tool.py`
- `tests/gateway/test_restart_resume_pending.py`

## Subsystem Methods

### Shared core across CLI and gateway

Borrow:

- one `AIAgent` loop reused across CLI and gateway contexts
- session source and cwd rules that differ by surface
- gateway daemon that owns messaging integrations while reusing the same core
  agent machinery

Use when:

- one harness must serve both local and remote surfaces

### Providerized subsystems

Borrow:

- memory, MCP, auxiliary LLM tasks, and external surfaces as swappable
  backends
- provider selection normalized through aliasing and fallback chains
- profile-scoped configuration rather than one global provider switch

Use when:

- backend replacement pressure is already real

Avoid when:

- the user only has one stable backend and no near-term replacement pressure

### Auxiliary routing and fallback chain

Borrow:

- shared auxiliary router for compression, vision, extraction, and other side
  tasks
- ordered fallback chain across main provider, aggregators, native providers,
  and custom endpoints
- interrupt protection for atomic auxiliary work such as compression

Use when:

- side tasks should not duplicate provider resolution logic
- fallback matters to continuity or uptime

### Layered memory with external provider

Borrow:

- external memory provider with tiered context loading
- automatic memory extraction on session commit
- multiple memory categories and URI-style resource organization
- memory as a true subsystem, not just transcript replay

Use when:

- memory spans more than one file or one markdown artifact
- semantic retrieval and ingestion are first-class requirements

### Skills and extension supply chain

Borrow:

- guarded skill fetch/install pipeline
- path validation, redirect checks, and safe bundle resolution
- skill distribution as a managed extension surface rather than manual copy

Use when:

- the agent must acquire and manage skills beyond one local repo

### MCP as long-lived runtime infrastructure

Borrow:

- background event loop for long-lived MCP connections
- per-server transport choices, reconnection, environment filtering, and
  parallel-call opt-in
- stderr redirection and sanitized errors so MCP servers do not corrupt the
  UI loop

Use when:

- MCP is operational infrastructure rather than a one-off tool call wrapper

### Restart and resume continuity

Borrow:

- `resume_pending` marker for interrupted sessions
- system note injection that distinguishes fresh interrupted work from stale
  history
- restart-resume behavior governed by session metadata, not only transcript
  tail shape

Use when:

- gateways may restart while conversations remain live

## Question Angles This Project Justifies

- What exactly is providerized here: model, memory, MCP, auxiliary tasks, or
  user surface?
- Does one shared core truly need to serve both CLI and gateway, or can those
  diverge?
- Is fallback part of correctness or only a best-effort convenience?
- Is memory one artifact, multiple tiers, or an external subsystem?
- If a restart interrupts a live session, what marker carries resume intent?

## Design Warnings

- Providerization adds indirection; do not introduce it before replacement
  pressure is real.
- Shared core is only worth it when the surfaces actually share semantics, not
  just branding.
