# Providerized Branch

## Open This Branch When

- multiple backends, shared core across surfaces, or fallback behavior create
  real architecture pressure

## Ask Like This

Do not ask "Should this be abstract?"

Useful options:

- `A. One provider, one surface, no abstraction yet`
- `B. Shared core with swappable model/provider only`
- `C. Shared core plus providerized memory, MCP, or auxiliary subsystems`
- `D. Multi-surface runtime with explicit fallback chain`

Then keep the branch open until at least these provider decisions are stable:

1. What is expected to change first: model, provider, memory backend,
   transport, or surface?
2. Does one harness core truly need to serve more than one surface?
3. Which fallback path matters to correctness or uptime?
4. Which abstraction would be premature right now?
5. What should stay concrete even if other layers are providerized?

Do not leave this branch until abstraction boundary, shared-core need, and
fallback posture are explicit.

## Borrow From

- `hermes-agent`
- `OpenHarness`
