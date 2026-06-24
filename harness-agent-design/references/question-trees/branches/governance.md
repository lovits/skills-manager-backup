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

Do not leave this branch until risk tiers, approval posture, and governance
ownership are explicit.

## Borrow From

- `claw-code`
- `opencode`
- `OpenHarness`
