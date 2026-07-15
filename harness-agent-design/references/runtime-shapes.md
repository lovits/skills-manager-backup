# Runtime Shapes

## Purpose

Use this file to classify the target agent before proposing an architecture.

## Common shapes

### Session CLI agent

Best for coding, research, and local task execution.

Use when:

- one user owns a session
- the loop can stay foregrounded
- approvals and tools happen inline

Avoid when:

- multiple channels must converge
- background events need reinjection

### Gateway agent

Best for always-on or multi-channel systems.

Use when:

- messages arrive from multiple sources
- heartbeat, cron, or webhooks must feed the same session loop
- identity, routing, or plugin management matters

Avoid when:

- the real problem is only local task execution

### Lightweight monolith

Best for MVPs and readable single-agent systems.

Use when:

- the loop is still evolving
- keeping the harness obvious matters more than extensibility

Avoid when:

- multiple replaceable providers are already required

### Dual-surface assistant

One harness core exposed through two surfaces, often CLI plus gateway.

Use when:

- a user needs both direct local control and remote/event surfaces

Primary-family hint:

- classify this as `providerized shared-core` only when the shared core itself
  is the design problem
- classify it as `always-on or gateway` when routing/session ownership is still
  the first unresolved risk

### Multi-agent runtime

Best when isolation or specialist roles create clear value.

Use when:

- subtasks pollute the parent context
- concurrent exploration matters
- planning and execution need different state scopes

Avoid when:

- multi-agent is being used to hide weak core-loop design

## Hybrid Prioritization Hints

- `always-on + coding`: settle session ownership and reinjection before coding
  mode details
- `coding + providerized`: settle execution boundary before backend abstraction
- `providerized + always-on`: settle shared-core first only when fallback or
  multi-surface reuse is already the dominant risk
- `lightweight + anything`: let anti-overbuild posture win ties unless the
  heavier shape is already forced by requirements
