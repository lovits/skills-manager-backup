# OpenHarness Methods

GitHub: [HKUDS/OpenHarness](https://github.com/HKUDS/OpenHarness)

## Why This Project Matters

OpenHarness is a major method source when the design problem is not just one
agent loop, but how to assemble a reusable harness platform from engine,
permissions, hooks, skills, plugins, MCP, memory, tasks, teams, UI, and
channel-facing overlays.

Its deeper lesson is that a harness can be:

- one shared query engine
- plus a governed assembly layer
- plus optional app overlays like `ohmo`

Do not reduce OpenHarness to "it has plugins and memory." It is most valuable
as an assembly-platform reference.

Its codegraph clusters around:

- `src/openharness/engine/query_engine.py`
- `src/openharness/plugins/loader.py`
- `src/openharness/tasks/manager.py`
- `src/openharness/swarm/team_lifecycle.py`
- `src/openharness/swarm/permission_sync.py`
- `src/openharness/memory/schema.py`
- `src/openharness/services/compact/__init__.py`
- `src/openharness/commands/registry.py`
- `ohmo/runtime.py`
- `ohmo/gateway/runtime.py`
- `README.md`

Read OpenHarness for subsystem questions like:

- how a reusable query engine should relate to app overlays
- what should be manifest-loaded versus hardcoded
- how memory can be typed, scoped, and freshness-aware
- how background tasks and teams become lifecycle subsystems
- how the runtime should expose plan/worktree/task/team/cron control surfaces
- how worker permissions can be synchronized across a swarm
- how an always-on personal assistant can reuse a general harness core

## Subsystem Methods

### Shared query engine as the core runtime

Borrow:

- one `QueryEngine` that owns history, tool-aware loop execution, usage
  tracking, and model runtime state
- system prompt, model, API client, permissions, hooks, and tool metadata all
  attached to one engine instead of scattered globals
- session memory preparation/extraction and auto-dream scheduling around the
  same core loop
- pre-turn session-memory preparation and post-turn checkpoint/extraction
  lifecycle owned by the engine instead of ad hoc middleware
- cost and usage ownership kept inside the same runtime core as messages and
  tools

Use when:

- several harness surfaces should share one conversation engine
- the runtime needs to update model, system prompt, permissions, or tool state
  without rebuilding the whole product
- you want a clear owner for "what happens when a user submits a message"

Avoid when:

- there is only one narrow app with no reuse pressure

Deep lesson:

- OpenHarness isolates the core conversation engine first
- then composes surrounding systems around it

### Harness core plus app overlay

Borrow:

- app overlays that supply workspace semantics, system prompt, memory backend,
  session backend, skill roots, and plugin roots
- a general harness runtime reused by a specialized assistant such as `ohmo`
- one pattern where the overlay changes context and storage, not the engine's
  basic semantics

Use when:

- one harness should back both a generic CLI and a domain/personal assistant
- the user wants reuse without collapsing all products into one config blob

Avoid when:

- the product surface is singular and unlikely to branch

### Plugin, skill, and hook assembly

Borrow:

- plugin manifests that can load skills, commands, agents, tools, hooks, and
  MCP servers
- user-root, project-root, and extra-root discovery with explicit trust posture
- project-local plugins gated by settings instead of silently trusted
- hook files and structured hook definitions as real extension seams
- on-demand skill loading and skill metadata parsing as first-class runtime
  behavior

Use when:

- extensibility is a real product requirement
- domain-specific behavior should plug in without forking the harness
- capabilities may arrive from several trusted roots but need one loader model

Avoid when:

- plugins are compensating for an unclear core architecture

Deep lesson:

- OpenHarness treats extension as an assembly problem with trust boundaries
- not as "scan folders and hope"

### Memory taxonomy and file-backed durable memory

Borrow:

- explicit `type` and `scope` taxonomies for durable memory
- `MEMORY.md` as index rather than memory body
- frontmatter-driven metadata with IDs, importance, source, tags, TTL, and
  signatures
- freshness/staleness language built into memory loading
- deduplication and entrypoint truncation rules
- separation among durable memory, session memory, and memory extraction

Use when:

- memory must be more structured than one monolithic note
- the harness needs different scopes such as private, project, and team
- memory freshness and organization matter to correctness

Avoid when:

- there is no real distinction between current session state and durable memory

Strong OpenHarness method: turn memory into a typed artifact system.

### Compaction, session memory, and extraction lifecycle

Borrow:

- explicit compaction service instead of ad hoc summary pasting
- file-backed session memory checkpoints
- optional post-turn extraction into durable memory
- pre-turn preparation of session memory metadata before compaction or model
  calls
- separate knobs for session memory, durable memory, and auto-extraction

Use when:

- long sessions need both continuity and durable knowledge capture
- the product needs a bridge between short-term and durable memory

Avoid when:

- memory is intentionally short-lived or entirely externalized

### Background tasks and local worker lifecycle

Borrow:

- `BackgroundTaskManager` for shell and agent subprocess tasks
- durable task records with status, command, cwd, logs, restart notice, and
  completion listeners
- JSON-line stdin framing for worker prompts
- restart behavior that distinguishes "task resumed" from "interactive context
  preserved"
- one manager for task create, stop, write, output read, and listener hooks

Use when:

- tasks may outlive one turn
- local agent or shell work should continue in the background
- the harness needs a coherent worker lifecycle rather than one-off subprocess
  calls

Avoid when:

- all operations are quick and strictly foreground

Deep lesson:

- OpenHarness treats background execution as a durable protocol
- not as "spawn a process and hope the user remembers what it was doing"

### Team lifecycle and permission synchronization

Borrow:

- persistent team metadata on disk, not just in RAM
- explicit team files, members, allowed paths, worktree paths, modes, and
  subscriptions
- file-based and mailbox-based permission sync protocols between leader and
  workers
- permission requests as structured artifacts with tool name, description,
  input, feedback, and policy updates
- serialized approval flows instead of silent worker autonomy

Use when:

- delegation is persistent enough to require audit and coordination
- leader/worker approval exchange must survive process boundaries
- team metadata should be inspectable and durable

Avoid when:

- delegation is only occasional and local

Deep lesson:

- OpenHarness treats swarm permissioning as a protocol
- not as a side-effect of "just ask the parent"

### Unified runtime control surface

Borrow:

- built-in tool registry that exposes filesystem, web, MCP, plan mode,
  worktree mode, cron, tasks, teams, skills, and agent control through one
  control surface
- command and tool surfaces that make runtime posture visible instead of hidden
  config edits
- the idea that operator control is part of the harness contract, not a
  separate admin shell

Use when:

- users or higher-level agents must steer runtime state explicitly
- plan/build posture, workspace isolation, background work, or team operations
  are part of the product
- one harness should expose several runtime surfaces without inventing a new
  admin API for each

Avoid when:

- there is only one narrow foreground interaction path

### Commands, plan/worktree modes, and runtime control surfaces

Borrow:

- a command registry as a first-class control surface
- explicit tools for entering plan mode, exiting it, entering worktrees, and
  manipulating tasks/teams/cron
- runtime behavior controlled through commands and tools, not hidden config
  mutations

Use when:

- the harness needs operator-facing control states
- plan/build or workspace-isolation posture should be visible and controllable

### Always-on overlay via ohmo

Borrow:

- `ohmo` as a workspace-scoped overlay with its own skills, plugins, memory,
  and sessions directories
- custom system prompt and memory backend while reusing the same runtime
- session backend rooted in the app workspace
- channel gateway runtime attached to the same assistant identity
- sender/thread-scoped session keys so group channels do not collapse into one
  undifferentiated conversation

Use when:

- a personal or channel-facing assistant should reuse a general harness core
- one runtime needs app-specific memory/session semantics

Avoid when:

- there is no always-on or personal-agent surface

Deep lesson:

- OpenHarness demonstrates how an app overlay can be real and opinionated
- without rewriting the shared harness core

## Question Angles This Project Justifies

- Is this a reusable harness core with overlays, or just one app pretending to
  be a platform?
- Which seams should be loadable: skills, hooks, plugins, agents, MCP,
  commands?
- Does memory need typed scopes and freshness policy?
- Does durable memory need signatures, truncation rules, and stale-memory
  language?
- Are background tasks and worker permissions first-class lifecycle concerns?
- Does the runtime need one unified control surface for plan/worktree/task/team
  state?
- Is there an app overlay like `ohmo` with its own workspace semantics?
- Are channel sessions keyed by sender/thread/chat scope or collapsed too
  early?
- Should delegation use durable team metadata and approval protocols?

## Design Warnings

- Do not import plugin/platform complexity if one local harness is enough.
- Do not add typed memory taxonomies before durable memory pressure is real.
- Do not treat teams, permissions, and background workers as optional details if
  the runtime already depends on them.
- Do not confuse "reusable core" with "everything must be generic from day
  one."
