# Coding Branch

## Open This Branch When

- code editing, shell execution, verification, or repo state are central

## Ask Like This

Start with loop ownership, not memory.

Useful options:

- `A. One owner loop`
- `B. Plan mode -> build mode`
- `C. Owner loop plus helper reviewer`
- `D. Full delegated coding runtime`

Keep the branch open until these decisions are stable:

1. What execution boundary matters most: read, write, shell, git, network?
2. Should planning be read-only?
3. What state must survive a long coding session?
4. How are verification signals fed back into the loop?
5. When is a helper justified instead of one stronger owner loop?
6. Does input need durable admission before it becomes model-visible history?
7. Should queued prompts wait for a safe provider-turn boundary?
8. Is permission posture a real state machine, or just a generic confirm
   prompt?
9. Should helper work be inspectable child sessions or invisible background
   work?
10. Which mechanism is actually next: permissions, todo/plan, subagent,
    compaction, memory, tasks, background work, or MCP?
11. If several mechanisms are desired, which one solves today's pressure and
    which ones should be deferred?

Do not leave this branch until loop shape, execution boundary, and verification
are explicit.

## Borrow From

- `claw-code`
- `opencode`
- `learn-claude-code`
