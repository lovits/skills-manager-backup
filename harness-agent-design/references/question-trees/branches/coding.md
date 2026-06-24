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

Then keep the branch open until at least these coding decisions are stable:

1. What execution boundary matters most: read, write, shell, git, network?
2. Should planning be read-only?
3. What state must survive a long coding session?
4. How are verification signals fed back into the loop?
5. When is a helper justified instead of one stronger owner loop?

Do not leave this branch until loop shape, execution boundary, and verification
posture are explicit.

## Borrow From

- `claw-code`
- `opencode`
- `learn-claude-code`
