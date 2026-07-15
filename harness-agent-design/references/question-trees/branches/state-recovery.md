# State and Recovery Branch

## Open This Branch When

- the runtime must survive restart, checkpoint, resume, compaction, or crash
- freshness, ownership markers, or durable state artifacts shape correctness

## Ask Like This

Start with what must still be true after interruption.

Useful options:

- `A. Restart from scratch is acceptable`
- `B. Resume one session with a small durable checkpoint`
- `C. Keep explicit session artifacts, compaction, and recovery rules`
- `D. Full state machine with lane, quota, or checkpoint status`

Then ask:

1. Which artifact must exist after a crash: transcript, checkpoint, todo,
   approval log, session snapshot, or lane marker?
2. Is resume keyed by workspace, user, entity, session key, or workflow job?
3. What must be durable state versus recomputable context?
4. Does compaction merely shorten context, or create a new checkpointed state?
5. What counts as freshness, staleness, suspension, or wedged state?
6. Does recovery mean retry, resume, reset, quarantine, or manual operator
   action?
7. Are child runs, tasks, or lanes independently recoverable?
8. Which state is user-readable, operator-readable, or machine-only?
9. Is restart-from-scratch safer than resume for any part of the runtime?
10. What failure snapshot or recovery artifact would prove the system can be
    trusted?

Do not leave this branch until state ownership, resume identity, and recovery
are explicit.

## Borrow From

- `OpenClaw`
- `claw-code`
- `opencode`
- `TradingAgents`
- `OpenHarness`
- `hermes-agent`
