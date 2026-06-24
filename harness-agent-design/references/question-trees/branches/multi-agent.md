# Multi-Agent Branch

## Open This Branch When

- one owner loop risks context pollution, specialist isolation is real, or
  concurrency pressure exists

## Ask Like This

Start by challenging whether the parent loop is insufficient.

Useful options:

- `A. No delegation`
- `B. One helper only when needed`
- `C. A few stable helper roles`
- `D. Full multi-agent runtime`

Then ask:

1. What exactly is the parent isolating?
2. What may a child never own?
3. Are child outputs advisory, executable, or state-mutating?
4. How are results reintegrated?
5. What concurrency limit or audit rule is required?

Do not leave this branch until parent ownership, child boundary, and
reintegration rules are explicit.

## Borrow From

- `OpenClaw`
- `learn-claude-code`
- `opencode`
