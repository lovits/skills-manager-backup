# opencode Methods

## Why This Project Matters

opencode is a strong source for coding harnesses where the hard question is
not merely "which tools exist?" but "which mode owns this turn, what
permissions follow from that mode, how do prompts become durable session
inputs, and when is a child runtime justified?"

Its codegraph clusters around:

- `packages/opencode/src/cli/cmd/run/runtime.ts`
- `packages/opencode/src/cli/cmd/run/permission.shared.ts`
- `packages/opencode/src/cli/cmd/run/subagent-data.ts`
- `packages/opencode/src/session/session.ts`
- `AGENTS.md`
- `CONTEXT.md`

## Subsystem Methods

### Family posture

Borrow:

- coding harness with an explicit `plan` agent versus `build` agent split
- one owner loop first, then one narrow helper surface (`general`) when search
  or decomposition pressure appears

Use when:

- the user needs read-only planning before mutation
- the core design problem is execution posture, not gateway routing

### Runtime and mode topology

Borrow:

- phase-aware mode split instead of one undifferentiated loop
- runtime state that tracks active variant, session, queue, history, and agent
  choice in one place
- footer or control-surface state that reflects whether the session is idle,
  running, waiting on permission, or waiting on a question

Use when:

- the harness must make plan/build, prompt, approval, and subagent surfaces
  visibly different

Avoid when:

- a single owner loop with one prompt box is enough

### Permission posture and execution boundary

Borrow:

- permission UI as an explicit state machine, not an ad hoc modal
- different defaults per mode, especially for bash and writes
- "allow once" versus scoped temporary "allow always" posture
- permission metadata that explains the action in user language before the
  runtime commits to it

Use when:

- the user wants plan mode to stay read-only
- file writes, shell, or external access need different approval paths

Avoid when:

- the runtime never crosses into mutation

### Durable prompt admission and session continuity

Borrow:

- durable prompt admission separate from model execution
- admitted input that may exist before it is promoted into visible session
  history
- safe provider-turn boundaries where newly admitted input becomes visible
- session drain semantics: one local execution span that promotes queued input
  and runs provider turns until continuation ends
- explicit `resume` and exact retry semantics on reused session identifiers

Use when:

- the system must survive restart or delayed processing without losing user
  inputs
- you need to reason about "pending but not yet model-visible" inputs

Avoid when:

- a short-lived local agent can stay ephemeral

### Context discipline

Borrow:

- context epoch thinking: baseline system context, updates, and refresh points
- mid-conversation system messages only when a context source actually changes
- separation between baseline context, session history, and tool output
- bounded tool output persisted to history plus spillover file for full output

Use when:

- the harness will accumulate long coding sessions with environment changes
- you need compaction without losing context provenance

Avoid when:

- the user just needs a short request-response loop

### Subagent boundary

Borrow:

- primary owner loop plus explicit child session detail panes
- child session tabs, status, queue, and tool-call summaries as inspectable
  state
- helper subagents for search and decomposition, not for core ownership

Use when:

- the main loop should retain ownership while one child isolates noisy work

Avoid when:

- subagents are being used to hide an unclear main loop

## Question Angles This Project Justifies

- Is this really one loop, or does it need a read-only `plan -> build`
  transition?
- Which permissions change with the phase: reads, writes, shell, git, network?
- Does the system need admitted-but-not-yet-visible inputs, or is immediate
  prompt handling enough?
- Is the child runtime advisory only, or does it own mutation?

## Design Warnings

- Do not import provider-turn and context-epoch machinery unless the user
  really has durable session pressure.
- Do not create `plan/build` theater when one well-governed owner loop would
  solve the problem.
