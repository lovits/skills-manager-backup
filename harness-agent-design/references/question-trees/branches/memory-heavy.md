# Memory Pressure Branch

## Open This Branch When

- the design shows real pressure from stable facts, cross-session continuity,
  shared memory, or user personalization

## Memory Pressures To Test

1. Cross-turn stable facts
2. Cross-session continuity
3. Team or shared memory
4. User personalization

## Ask Like This

Do not ask "Should it have memory?"

Useful options:

- `A. No durable memory, keep it session-local`
- `B. Session history plus a small restart-safe memory note`
- `C. Personal and project-scoped memory overlay`
- `D. Layered memory subsystem with retrieval, promotion, and lifecycle hooks`

Keep the branch open until these decisions are stable:

1. Which pressure is actually present: stable facts, cross-session continuity,
   shared memory, or personalization?
2. Which facts must survive restart, and which should stay session-local?
3. Is memory a small artifact, a scoped overlay, or a retrieval subsystem?
4. Is memory host-owned with explicit hooks, or left to ad hoc prompt logic?
5. Who writes memory, who reads it, and who may correct stale memory?
6. Does memory need prefetch-before-turn and sync-after-turn behavior?
7. Is this generic memory, or outcome-reflection memory that writes only after
   real feedback or results arrive?
8. Does durable memory need typed artifacts, signatures, freshness text, or
   deduplication rules?
9. What stale-memory failure is dangerous enough to shape the design?
10. Must memory survive session switches, compaction, delegation, or restart?

Do not leave this branch until memory scope, write/read ownership, and
staleness risk are explicit.

## Borrow From

- `hermes-agent`
- `OpenHarness`
- `nanobot`
- `TradingAgents`
