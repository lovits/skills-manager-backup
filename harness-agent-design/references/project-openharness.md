# OpenHarness Methods

## Why This Project Matters

OpenHarness is a strong source when the design problem is harness assembly:
tool-use cycle, skills, hooks, permissions, memory artifacts, session resume,
background task lifecycle, team coordination, and app overlays on top of one
shared runtime.

Its codegraph clusters around:

- `src/openharness/plugins/loader.py`
- `src/openharness/tasks/manager.py`
- `src/openharness/swarm/team_lifecycle.py`
- `src/openharness/swarm/permission_sync.py`
- `src/openharness/memory/schema.py`
- `ohmo/runtime.py`
- `ohmo/session_storage.py`
- `ohmo/gateway/runtime.py`

## Subsystem Methods

### Harness core plus app overlay

Borrow:

- one general harness runtime that can be wrapped by a more specialized app
- app overlay supplying system prompt, workspace roots, skills, plugins, and
  session backend instead of rewriting the engine
- explicit distinction between core harness concerns and app semantics

Use when:

- one reusable harness should back both a plain CLI and a more personal or
  channel-facing assistant

Avoid when:

- there is only one narrow runtime and no reuse pressure

### Tool cycle and background work

Borrow:

- explicit background task manager for shell and agent tasks
- durable task records with status, output, restart notice, and completion
  listeners
- foreground loop separated from long-running worker processes

Use when:

- tasks may outlive one turn
- the agent needs background execution without losing operator control

Avoid when:

- all work finishes inside a single synchronous turn

### Plugin, skill, and hook assembly

Borrow:

- plugin discovery from user roots, project roots, and extra roots
- plugin manifests that can load skills, commands, agents, tools, hooks, and
  MCP configuration
- hooks and skills as loadable seams, not hardcoded branches
- project-local plugins gated by trust settings

Use when:

- extensibility is required but should stay composable
- the user wants to add domain behavior without rewriting the loop

Avoid when:

- extension seams are compensating for an unclear core loop

### Memory taxonomy and artifact policy

Borrow:

- `MEMORY.md` as an index plus typed memory files
- memory frontmatter with `type` and `scope`
- freshness warnings, truncation rules, and staleness-aware language
- memory signatures for deduplication

Use when:

- durable memory needs explicit structure rather than one opaque blob
- the agent must distinguish private, project, and team memory pressure

Avoid when:

- there is no real difference between session state and durable memory

### Session snapshots and resume

Borrow:

- snapshot backend rooted in app workspace
- latest snapshot plus session-key-indexed latest snapshot
- transcript export and restore
- session bundle reuse when the session key and cwd still match

Use when:

- the harness must resume long sessions after restart
- one chat thread or user channel should continue the same runtime identity

Avoid when:

- the runtime is intentionally ephemeral

### Permissions, team lifecycle, and coordination

Borrow:

- permission modes plus path and command rules
- permission sync across teammates using pending and resolved artifacts
- explicit team lifecycle metadata: members, allowed paths, team files, and
  team registry state
- serialized permission exchange between parent and child workers

Use when:

- multi-agent work must stay auditable
- approvals must cross process or teammate boundaries

Avoid when:

- there is no real delegated execution

### Always-on channel overlay via ohmo

Borrow:

- one runtime bundle per session key
- gateway/session pool that restores prior snapshot by session key
- channel-scoped command handling that updates provider/model or session
  behavior without rebuilding the whole core
- app-specific memory and session directories separate from project memory

Use when:

- the assistant is always-on in chat channels
- the same conversation key must resume the same runtime bundle

Avoid when:

- a plain local harness is enough

## Question Angles This Project Justifies

- Is this one reusable harness with an app overlay, or just one app-specific
  runtime?
- Which seams should be loadable: skills, hooks, plugins, agents, MCP?
- Does memory need typed scopes and freshness policy, or just transcript
  history?
- Are background tasks and team permissions first-class lifecycle concerns?
- If this is always-on, what is the session key and where are snapshots rooted?

## Design Warnings

- Do not import channel-app complexity if the user only needs a plain harness.
- Do not add plugins, teams, or memory taxonomies before the minimum loop is
  stable.
