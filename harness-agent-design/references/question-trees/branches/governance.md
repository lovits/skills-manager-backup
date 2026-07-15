# Governance Branch

## Open This Branch When

- tool risk, permissions, approvals, or auditability are core design concerns

## Ask Like This

Start with what must never happen.

Useful options:

- `A. Light warning only`
- `B. Approval before high-risk actions`
- `C. Phase-aware permissions`
- `D. Auditable governance with hooks and logs`

Then ask:

1. Which actions are read-only, mutating, or high-risk?
2. Should planning and execution have different permissions?
3. Where does policy live: inside tools or in a central governance layer?
4. What must be durable or inspectable after a risky action?
5. Are access control and visible reply policy part of governance, or treated
   as channel-specific afterthoughts?
6. Which plugin, bundle, MCP, or external runtime sources are trusted enough to
   load?
7. Is side-effect safety enforced only at the tool layer, or also at the
   gateway / session / routing layer?
8. Do different runtime modes carry different approval defaults and grant
   scopes?
9. Can external runtimes participate through a bounded relay, and if so what
   event allowlist, TTL, or attribution is required?
10. Do unattended jobs, background reviews, or remote sessions need stricter
    toolset narrowing than interactive mode?
11. Is team or worker approval a casual chat exchange, or a durable protocol
    with inspectable artifacts?
12. Are repeated failed tool calls merely noisy, or should loop guardrails warn
    or halt?

Do not leave this branch until risk tiers, approval posture, and governance
ownership are explicit.

## Borrow From

- `claw-code`
- `opencode`
- `OpenHarness`
- `OpenClaw`
- `hermes-agent`
