# Background Automation Branch

## Open This Branch When

- tasks, cron, heartbeat, unattended execution, or worker lifecycle are real
- some work may continue when no user is actively watching the runtime

## Ask Like This

Start with what keeps running after the turn ends.

Useful options:

- `A. No background work`
- `B. One foreground loop plus manual re-entry later`
- `C. Background tasks or workers with durable task records`
- `D. Scheduled or unattended runtime with explicit safeguards`

Then ask:

1. What keeps running after the current user turn: shell task, agent task,
   cron, heartbeat, queued event drain, or delegated worker?
2. Is this the same runtime surface as the interactive assistant, or a separate
   execution lane?
3. What durable task record, log, or status artifact must exist?
4. Does restart preserve interactive context, only task identity, or neither?
5. What approval, tool narrowing, or delivery policy changes in unattended
   mode?
6. Is scheduling time-based, event-based, heartbeat-based, or operator-fired?
7. What wakes the user back up: notification, message send, summary artifact,
   or manual inspection?
8. Which failures should auto-retry, which should mark the task stuck, and
   which should stop the lane?
9. Does automation reuse the same memory/session context, or write back only
   bounded artifacts?
10. What would make this background path too heavy for v1?

Do not leave this branch until worker lifecycle, unattended safeguards, and
operator wake-up are explicit.

## Borrow From

- `OpenHarness`
- `hermes-agent`
- `OpenClaw`
- `nanobot`
- `claw-code`
