# Providerized Branch

## Open This Branch When

- multiple backends, shared core across surfaces, or fallback behavior create
  real architecture pressure
- auxiliary tasks, memory backend, MCP runtime, or unattended execution may
  need different runtime treatment from the main loop

## Ask Like This

Do not ask "Should this be abstract?"

Useful options:

- `A. One provider, one surface, no abstraction yet`
- `B. Shared core with swappable model/provider only`
- `C. Shared core plus providerized memory, MCP, or auxiliary subsystems`
- `D. Multi-surface or unattended runtime with explicit fallback chain`

Keep the branch open until these decisions are stable:

1. What is expected to change first: model, provider, memory backend,
   transport, or surface?
2. Does one harness core truly need to serve more than one surface?
3. Are auxiliary tasks allowed to use a different provider chain from the main
   turn loop?
4. Is fallback only for the main answer path, or also for compression,
   retrieval, MCP-backed work, and background jobs?
5. Does the runtime need a capability registry for structured output, tool
   choice, or reasoning quirks across models?
6. Do different stages or subsystems need different reasoning tiers or model
   families?
7. Which fallback path matters to correctness or uptime?
8. Which abstraction would be premature right now?
9. What should stay concrete even if other layers are providerized?
10. Does unattended execution create stricter timeout, delivery, or provider
    rules than interactive mode?

Do not leave this branch until abstraction boundary, shared-core need, and
fallback are explicit.

## Borrow From

- `hermes-agent`
- `TradingAgents`
- `OpenHarness`
