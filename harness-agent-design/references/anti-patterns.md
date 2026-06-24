# Anti-Patterns

## Purpose

Use this file to push back on weak architectures.

## Common anti-patterns

### Feature list as architecture

Symptom:

- user lists memory, tools, planner, cron, and workers without explaining why

Correction:

- tie each subsystem to a specific requirement or failure mode

### Premature gateway

Symptom:

- adding a control plane before proving a single loop

Correction:

- keep a session loop until multi-channel or always-on pressure appears

### Premature multi-agent

Symptom:

- multiple agents are added to compensate for a vague parent workflow

Correction:

- first define what the parent owns and what a child isolates

### Hidden memory semantics

Symptom:

- "the agent should remember things" without retention rules or artifacts

Correction:

- define session memory, durable memory, and retrieval triggers separately

### Ungoverned tool surface

Symptom:

- tools execute directly with no approval or policy boundary

Correction:

- centralize tool gating, approval, and audit logic

### Missing recovery story

Symptom:

- long-running agent design has no restart, replay, or resume behavior

Correction:

- define durable state, failure checkpoints, and recovery operator flow
