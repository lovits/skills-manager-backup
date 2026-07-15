# Claw Code Notes

GitHub: [ultraworkers/claw-code](https://github.com/ultraworkers/claw-code)

## Why This Project Matters

claw-code is a strong source for coding harnesses with centralized runtime
governance. Its value is not only "coding agent" but the way it keeps
permissions, hooks, policy, session control, recovery, and task freshness in
one runtime-owned layer.

For this skill, claw-code should not be reduced to "it has approvals" or "it
has hooks." The deeper lesson is that claw-code treats the harness as an
operating system for agent execution:

- one runtime owns execution posture
- one policy layer interprets what is allowed next
- one hook layer intercepts tool activity
- one session layer names and resumes work safely
- one recovery layer handles known failures before escalation
- one task layer keeps delegated lanes observable and fresh

That makes it especially valuable when the user is designing a serious coding
runtime rather than a lightweight assistant.

Its codegraph clusters around:

- `rust/crates/runtime/src/config.rs`
- `rust/crates/runtime/src/policy_engine.rs`
- `rust/crates/runtime/src/hooks.rs`
- `rust/crates/runtime/src/session_control.rs`
- `rust/crates/runtime/src/recovery_recipes.rs`
- `rust/crates/runtime/src/task_registry.rs`

Read claw-code as a method source for at least these subsystem questions:

- where should runtime governance live
- how should permissions and policy interact
- what belongs in hooks versus tools
- how should sessions be named, resumed, and isolated
- which failures deserve automatic recovery logic
- how should child lanes stay visible and fresh
- how should provider and MCP posture stay under the same runtime authority

## Subsystem Methods

### Centralized runtime governance

Borrow:

- one runtime feature config that owns hooks, plugins, MCP, provider fallback,
  permission mode, rules import, and sandbox posture
- config inspection and precedence reporting instead of hidden runtime state
- explicit runtime-level authority over execution posture rather than letting
  each tool or plugin invent its own safety model
- one place to answer "what is this harness allowed to do right now" across
  shell, file mutation, MCP, plugins, and delegation

Use when:

- the user needs one authority for execution posture
- the design is drifting toward tool-by-tool policy sprawl
- the harness may eventually have multiple extension surfaces that still need a
  single operating contract

This is one of claw-code's biggest contributions: it shows that mature coding
harnesses stop being "a loop plus some tools" and become a governed runtime.

### Policy engine above tools

Borrow:

- policy conditions and actions as explicit data, not scattered `if` blocks
- lane context including green level, freshness, blockers, review status,
  approval token, and retry state
- policy decisions that can retry, escalate, reconcile, merge, or require
  approval
- policy as a decision layer that reads runtime state, not just a permission
  prompt attached to one command
- auditable next-step logic such as "retry once", "request approval", "pause
  for reconciliation", or "refuse until blocker clears"
- explicit distinction between current lane state and allowed transition

Use when:

- the runtime needs auditable decisions about what may happen next
- the user is designing a harness where governance must remain legible under
  failures and retries
- approval is not binary and must depend on lane state, freshness, or risk

Avoid when:

- a lightweight harness only needs simple approvals

Deep lesson:

- claw-code does not treat policy as UX chrome
- it treats policy as runtime transition logic

### Hooks as governed interception

Borrow:

- pre-tool and post-tool hooks with progress reporting
- hook-level permission overrides and revised tool input
- abortable hook execution with explicit started/completed/cancelled events
- hooks as first-class runtime intervention points rather than ad hoc wrappers
- hook execution telemetry so the operator can tell whether enforcement,
  mutation, or annotation happened
- the idea that hooks can enrich or stop a tool call before side effects happen
- a clear boundary between "hook changed the call" and "tool executed the call"

Use when:

- policy should intervene before or after tools without moving logic into each
  tool implementation
- compliance, path rules, sanitization, or audit logging should stay outside
  the tool body
- the harness may need to evolve intervention behavior without rewriting the
  tool set

This matters because many weak harnesses bury governance inside tool adapters.
claw-code shows a cleaner split: hooks intercept, policy decides, tools do work.

### Tool-side-effect governance

Borrow:

- differentiated posture for read versus write versus execute versus networked
  operations
- governance that follows the action radius of the tool call, not just the
  name of the tool
- side effects treated as a runtime concern, not only as model prompting

Use when:

- the user is building a coding agent that edits files, runs tests, mutates
  branches, or talks to external services
- "can the agent call tools?" is too shallow and the real issue is "which
  side effects are acceptable under which state"

### Session control and namespace discipline

Borrow:

- per-workspace session stores
- session reference aliases such as latest, with exclusion of the current
  empty session
- managed session paths and workspace fingerprints to avoid collisions
- resume semantics that are careful about namespace, workspace, and recency
- session identity that belongs to the runtime, not to arbitrary UI labels
- safe handling of "latest session" so users do not resume the wrong context

Use when:

- session resume must be safe across multiple workspaces or processes
- the harness may be invoked repeatedly from the same machine across many repos
- users need convenience aliases without unsafe ambiguity

Deep lesson:

- claw-code treats session continuity as an operational correctness problem
- not just as "save chat history somewhere"

### Recovery recipes

Borrow:

- named failure scenarios with one automatic recovery pass before escalation
- recovery ledgers that track attempts, remaining budget, and escalation
  reason
- different recipes for trust prompts, stale branches, MCP failures, plugin
  startup, provider failures, and prompt misdelivery
- failure classes modeled explicitly instead of one giant generic retry loop
- bounded retry budgets attached to scenario types
- escalation reasons preserved so the runtime can explain what was attempted
- the idea that recovery belongs in harness design, not as an afterthought

Use when:

- the harness is long-running enough that known failures need first-class
  recovery logic
- the user wants robust automation but not silent looping
- the runtime has recurring operational failure modes worth encoding once

This is a major method contribution from claw-code. It helps the skill ask:

- which failures are common enough to deserve recipes
- which ones should be retried once
- which ones should immediately surface to the user

### Task registry and lane freshness

Borrow:

- explicit task lifecycle states
- heartbeat-based freshness for running lanes
- generated lane boards separating active, blocked, and finished work
- worker visibility that does not depend on reading raw logs
- freshness and staleness as modeled runtime state
- task metadata rich enough to support governance and recovery decisions
- lane dashboards that let an operator see "alive, blocked, stale, done"

Use when:

- subagent or worker tasks need operational visibility
- the design includes helper lanes, background workers, or parallel reviews
- blocked versus stale versus finished states matter operationally

This is where claw-code goes beyond many coding harnesses. It does not stop at
"spawn helper"; it asks how helpers remain governable over time.

### Extension supply under runtime control

Borrow:

- plugins, hooks, MCP, rules, and provider behavior all folded into one runtime
  control surface
- extension loading that still respects central governance instead of bypassing
  it
- the notion that every extension surface should inherit the runtime's safety
  posture

Use when:

- the user wants a plugin-capable coding harness without surrendering runtime
  authority
- MCP and plugin growth threaten to fragment execution policy

### Approval and escalation posture

Borrow:

- approval tokens or approval state that participates in policy decisions
- escalation paths that distinguish recoverable runtime issues from actions
  needing human confirmation
- explicit runtime logic for when to stop automation and surface a decision

Use when:

- the agent can make meaningful changes but some transitions still need human
  signoff
- the user wants autonomy with guardrails instead of constant blocking prompts

### Provider and backend governance

Borrow:

- provider fallback posture kept inside the same runtime contract as tools and
  permissions
- backend failure handled as a governed runtime event, not just a model error
- fallback policy attached to operator-configured runtime state

Use when:

- the harness may switch or fall back across providers during real work
- backend reliability affects correctness, continuity, or trust

## What To Extract From Claw Code During Interview

If the target agent resembles a serious coding harness, this project justifies
asking more than basic loop questions. It justifies asking:

- where is the single source of truth for execution posture
- is policy encoded as explicit runtime transitions or scattered ad hoc checks
- which tool actions are high-side-effect and how are they governed
- should hooks intercept, sanitize, annotate, or veto tool calls
- how is session identity made safe across repos and resumes
- what named failures deserve one bounded automatic recovery pass
- if there are helper lanes, how are freshness and blocked state observed
- do plugins, MCP, and provider fallback all remain under one runtime contract

## Question Angles This Project Justifies

- Should governance live centrally in the runtime, or is it leaking into
  tools?
- Does this system need policy conditions and recoveries, or only simple
  approvals?
- What failures deserve one automatic recipe before escalation?
- If there are child lanes, how will freshness and blocked state be observed?
- Should session resume be convenience-first or safety-first?
- Are hooks just callbacks, or do they participate in runtime governance?
- Is provider fallback a hidden backend detail, or part of harness design?

## Design Warnings

- Do not import all of claw-code's governance machinery into a simple chat or
  research agent.
- Do not scatter policy across tools if the runtime can own it once.
- Do not copy claw-code as a whole product shape when the user only needs one
  or two of its methods.
- Do not mistake "more runtime machinery" for "better harness." claw-code is a
  source of methods under high governance pressure, not a default answer.

## Summary Read

Use claw-code deeply when the design pressure sounds like:

- coding agent
- meaningful side effects
- policy and auditability matter
- helper lanes may exist
- recovery matters
- runtime extensions must stay governed

Use it lightly when the design pressure sounds like:

- lightweight assistant
- mostly advisory system
- little or no side effect radius
- no real delegation or recovery burden
