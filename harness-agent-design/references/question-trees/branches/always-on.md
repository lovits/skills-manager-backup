# Always-On or Gateway Branch

## Open This Branch When

- the agent receives external events, runs in multiple channels, or must keep
  acting between user turns
- restart recovery, queued events, or unattended background delivery are part
  of the runtime

## Ask Like This

Start with routing and session continuity, not tools.

Useful options:

- `A. Still one local session loop`
- `B. One gateway feeding one session loop`
- `C. Gateway plus channel-specific session rules`
- `D. Full control plane with delegates`

Keep the branch open until these decisions are stable:

1. Which channels or event sources exist?
2. Do events re-enter one session or create new sessions?
3. What background tasks or timers are real?
4. What session state must be inspectable and recoverable?
5. After a crash, does the runtime reset, resume, or mark the session pending?
6. Is reply routing deterministic and host-owned, or is the model being asked
   to choose a channel?
7. Which chats share one session and which must be isolated?
8. Which events must re-enter the same session identity, and which should open
   a new one?
9. Which background activity counts as true session freshness, and which should
   not extend the conversation?
10. Should ambient room events stay quiet until a message-send tool decides to
   speak?
11. What pairing, allowlist, or mention-gating posture is required?
12. Is this really one assistant across many surfaces, or many isolated
    personas on one control plane?
13. Are scheduled or unattended jobs using the same runtime, or a separate
    execution surface with different safeguards?

Do not leave this branch until routing, session ownership, and recovery are
explicit.

## Borrow From

- `OpenClaw`
- `OpenHarness`
- `ohmo`
- `hermes-agent`
