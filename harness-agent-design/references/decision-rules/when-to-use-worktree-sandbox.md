# When to Use Worktree or Sandbox

## Introduce isolation when

- the agent edits code or other sensitive artifacts
- experiments or child tasks should not pollute the parent workspace
- destructive actions need reversible containment

## Avoid it when

- the task is read-only
- isolation overhead would exceed the risk

## Preferred shape

- worktree isolation for code changes
- sandbox or approval boundary for risky execution

## Provenance

- learn-harness-engineering worktree isolation
- coding-harness governance patterns
