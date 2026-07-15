# opencode Methods

GitHub: [anomalyco/opencode](https://github.com/anomalyco/opencode)

## Why This Project Matters

opencode is a strong source for coding harnesses where the hard question is
not merely "which tools exist?" but:

- which mode owns this turn
- what permissions follow from that mode
- how prompts become durable session inputs
- when queued input becomes model-visible
- how long coding sessions refresh context
- when a child runtime is justified and how it stays inspectable

Do not reduce opencode to "plan/build split." Its deeper lesson is a visibly
staged coding runtime with explicit turn ownership:

- the runtime knows which agent or mode owns the turn
- permission posture follows from that mode rather than from vague prompting
- user input can be admitted durably before it becomes replayable history
- context refresh has named boundaries instead of hidden prompt mutation
- helper runs are visible child sessions, not opaque background magic

This makes it a strong source for coding harnesses that need staged authority,
durable input handling, and inspectable helper work.

Its codegraph clusters around:

- `packages/opencode/src/cli/cmd/run/runtime.ts`
- `packages/opencode/src/cli/cmd/run/runtime.lifecycle.ts`
- `packages/opencode/src/cli/cmd/run/runtime.queue.ts`
- `packages/opencode/src/cli/cmd/run/permission.shared.ts`
- `packages/opencode/src/cli/cmd/run/subagent-data.ts`
- `packages/opencode/src/cli/cmd/run/variant.shared.ts`
- `packages/opencode/src/session/session.ts`
- `packages/opencode/src/session/prompt.ts`
- `packages/opencode/src/session/run-state.ts`
- `AGENTS.md`
- `CONTEXT.md`

Read opencode for subsystem questions like:

- when one coding runtime should split into explicit modes
- how permission posture should follow phase, not just tool name
- how user input should be admitted durably before provider execution
- how long-running coding sessions refresh context without corrupting history
- when helper sessions should exist and how they remain inspectable
- how queue, interrupt, and busy semantics stay explicit during long runs

## Subsystem Methods

### Family posture

Borrow:

- coding harness with an explicit `plan` agent versus `build` agent split
- one owner loop first, then one narrow helper surface (`general`) when search
  or decomposition pressure appears
- a visible distinction between "analyze first" and "execute now"
- skepticism toward multi-agent theater when one owner loop would do

Use when:

- the user needs read-only planning before mutation
- the core design problem is execution posture, not gateway routing
- the runtime should make "not yet executing" legible to the user

Deep lesson:

- opencode does not split modes for ceremony
- it splits modes so authority and permission posture stay visible

### Runtime and mode topology

Borrow:

- phase-aware mode split instead of one undifferentiated loop
- runtime state that tracks active variant, session, queue, history, and agent
  choice in one place
- footer or control-surface state that reflects whether the session is idle,
  running, waiting on permission, or waiting on a question
- a top-level orchestrator that owns lifecycle, stream transport, and prompt
  queue together
- explicit runtime state for busy, waiting, switching, and queued work
- persisted model variant selection as operator state rather than hidden prompt
  detail

Use when:

- the harness must make plan/build, prompt, approval, and subagent surfaces
  visibly different
- the user wants the runtime to expose what phase it is in, not just emit text
- the system needs explicit busy/idle/switching semantics during long sessions

Avoid when:

- a single owner loop with one prompt box is enough

Major opencode method: the harness surface should explain runtime state, not
hide it behind one transcript.

### Permission posture and execution boundary

Borrow:

- permission UI as an explicit state machine, not an ad hoc modal
- different defaults per mode, especially for bash and writes
- "allow once" versus temporary "allow always" posture
- permission metadata that explains the action in user language before the
  runtime commits to it
- transition-specific approval steps such as confirm/cancel for wider grants
- differentiated permission semantics for shell, writes, external directories,
  and repeated-failure continuation

Use when:

- the user wants plan mode to stay read-only
- file writes, shell, or external access need different approval paths
- the runtime must distinguish "read", "mutate", "repeat risky loop", and
  "leave safe workspace" concerns
- the approval surface itself is part of product trust

Avoid when:

- the runtime never crosses into mutation

Deep lesson:

- opencode treats permission as a real harness state machine
- not a one-off confirmation popup

### Durable prompt admission and session continuity

Borrow:

- durable prompt admission separate from model execution
- admitted input that may exist before it is promoted into visible session
  history
- safe provider-turn boundaries where newly admitted input becomes visible
- session drain semantics: one local execution span that promotes queued input
  and runs provider turns until continuation ends
- explicit `resume` and exact retry semantics on reused session identifiers
- separation between "pending in inbox" and "already model-visible"
- replayable pending input that survives restart or delayed execution
- queue semantics where not every input is promoted immediately

Use when:

- the system must survive restart or delayed processing without losing user
  inputs
- you need to reason about "pending but not yet model-visible" inputs
- long-running coding sessions may have turns queued behind active work
- the user wants durable correctness under retries rather than best-effort chat

Avoid when:

- a short-lived local agent can stay ephemeral

This is one of opencode's strongest design contributions. It helps the skill
ask:

- do you need admitted-but-not-yet-visible user inputs
- when should queued user intent become model-visible
- does the runtime need exact retry or only "ask again"

### Prompt queue, busy state, and interruption discipline

Borrow:

- serial prompt queue with editable local queued prompts before their turn
- explicit busy-versus-idle runner ownership per session
- cancel paths that clear session-local work and child/background jobs together
- `/new` or equivalent control commands handled by runtime queue semantics
- wall-clock turn duration and queue length as runtime-visible state

Use when:

- multiple user prompts may arrive during active coding work
- the runtime needs a clean story for interruption and next-turn ordering
- user control commands should be runtime semantics, not just parsed text

### Context discipline

Borrow:

- context epoch thinking: baseline system context, updates, and refresh points
- mid-conversation system messages only when a context source actually changes
- separation between baseline context, session history, and tool output
- bounded tool output persisted to history plus spillover file for full output
- durable runtime language such as admitted prompt, promotion, safe
  provider-turn boundary, and context snapshot
- context sampled lazily at safe turn boundaries, not pushed asynchronously
- compaction or incompatible transitions causing a fresh baseline
- system context as typed sources rather than one monolithic prompt string

Use when:

- the harness will accumulate long coding sessions with environment changes
- you need compaction without losing context provenance
- multiple instruction or environment sources may change during one session
- the user needs strong reasoning about what the model actually saw when

Avoid when:

- the user just needs a short request-response loop

Deep lesson:

- opencode names context transitions explicitly
- that makes long coding sessions governable instead of magical

### Subagent boundary

Borrow:

- primary owner loop plus explicit child session detail panes
- child session tabs, status, queue, and tool-call summaries as inspectable
  state
- helper subagents for search and decomposition, not for core ownership
- child activity surfaced in the main runtime rather than hidden
- helper runs as bounded sessions with their own state instead of opaque tasks
- session-store updates that preserve parent ownership while exposing child work

Use when:

- the main loop should retain ownership while one child isolates noisy work
- users need to inspect helper progress, tool calls, or output before trusting
  it
- search or decomposition pressure is real but should not fracture the main
  runtime

Avoid when:

- subagents are being used to hide an unclear main loop

### Variant, model, and operator posture

Borrow:

- operator-visible model and variant cycling
- saved variant preference across sessions
- variant selection resolved from CLI input, saved preference, then session
  history
- model label and active variant as part of runtime-visible control state

Use when:

- the user should be able to steer reasoning effort or model flavor explicitly
- one coding harness must expose more than one execution posture without
  changing the whole architecture

### UI/runtime coherence

Borrow:

- one lifecycle layer that keeps renderer, footer, stream transport, and queue
  coherent
- exit/new/session-switch behavior treated as runtime transitions
- operational footers that expose permissions, questions, subagents, queued
  work, and mode

Use when:

- the coding harness has a real interactive surface
- the product should show operational state instead of letting it disappear
  into the transcript

## Question Angles This Project Justifies

- Is this really one loop, or does it need a read-only `plan -> build`
  transition?
- Which permissions change with the phase: reads, writes, shell, git, network?
- Does the system need admitted-but-not-yet-visible inputs, or is immediate
  prompt handling enough?
- Is the child runtime advisory only, or does it own mutation?
- Should queued user prompts wait until a safe provider-turn boundary?
- Does the runtime need exact retry semantics on durable prompt identity?
- Which context changes require a fresh baseline versus a chronological update?
- Should helper runs be inspectable child sessions or invisible background
  work?
- Does the UI/runtime need to expose busy, waiting, switching, and variant
  state explicitly?

## Design Warnings

- Do not import provider-turn and context-epoch machinery unless the user
  really has durable session pressure.
- Do not create `plan/build` theater when one well-governed owner loop would
  solve the problem.
- Do not use subagents to compensate for missing parent-loop clarity.
- Do not let permission posture stay implicit when the whole point is staged
  execution authority.

## Summary Read

Use opencode deeply when the design pressure sounds like:

- coding harness
- visible plan/build posture
- durable prompt handling
- explicit permission state
- long sessions with context refresh pressure
- one owner plus inspectable helper sessions

Use it lightly when the design pressure sounds like:

- one short coding loop
- little queue or retry complexity
- no need for explicit mode or permission staging
