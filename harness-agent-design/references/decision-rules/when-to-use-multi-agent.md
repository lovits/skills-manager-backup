# When to Use Multi-Agent

## Introduce multi-agent when

- subtasks would pollute the parent context
- specialist roles materially improve quality
- concurrent exploration or review is required

## Avoid it when

- the parent loop is still vague
- role boundaries are hand-wavy
- sequential work would be simpler and sufficient

## Preferred shape

- one clear parent owner
- bounded child scope
- explicit reintegration path

## Provenance

- openclaw delegate architecture
- learn-harness-engineering subagents
