# Always-On or Gateway Branch

## Open This Branch When

- the agent receives external events, runs in multiple channels, or must keep
  acting between user turns

## Ask Like This

Start with routing and session continuity, not tools.

Useful options:

- `A. Still one local session loop`
- `B. One gateway feeding one session loop`
- `C. Gateway plus channel-specific session rules`
- `D. Full control plane with delegates`

Then keep the branch open until at least these always-on decisions are stable:

1. Which channels or event sources exist?
2. Do events re-enter one session or create new sessions?
3. What background tasks or timers are real?
4. What session state must be inspectable and recoverable?
5. What operator action is needed after a crash or interruption?

Do not leave this branch until routing, session ownership, and recovery posture
are explicit.

## Borrow From

- `OpenClaw`
- `OpenHarness`
- `ohmo`
