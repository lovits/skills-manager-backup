# Multi-Agent Branch

## Open This Branch When

- one owner loop risks context pollution, specialist isolation is real, or
  concurrency pressure exists

## Ask Like This

Start by challenging whether the parent loop is insufficient.

Useful options:

- `A. No delegation`
- `B. One helper only when needed`
- `C. A few stable helper roles or a fixed specialist pipeline`
- `D. Full multi-agent runtime`

Then ask:

1. What exactly is the parent isolating?
2. Is this actually dynamic child delegation, or a stable role graph with
   specialist stages?
3. Do specialist stages need different tool surfaces or the same one?
4. What may a child, specialist, or judge stage never own?
5. Are child outputs advisory, executable, typed-stage artifacts, or
   state-mutating?
6. How are results reintegrated: summary, structured state, debate history, or
   approval artifact?
7. What checkpoint, resume, or audit rule is required?
8. Is this actually child delegation, or should separate channels/accounts bind
   to separate isolated personas instead?
9. Which state belongs to one routed persona versus one spawned child run?
10. If there is a judge/manager stage, what exactly is it approving or
    arbitrating?
11. Does the graph need different reasoning tiers for early specialists versus
    final synthesis?

Do not leave this branch until parent ownership, child boundary, and
reintegration are explicit.

## Borrow From

- `TradingAgents`
- `OpenClaw`
- `learn-claude-code`
- `opencode`
