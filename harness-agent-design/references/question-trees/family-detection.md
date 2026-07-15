# Harness Family Detection

## Goal

Classify the likely family early so later questions are branch-driven, not
checklist-driven.

## Expected Depth

- Give the family read early.
- If the case is clean, move on after one confirming question or synthesis.
- If the case is mixed or ambiguous, ask 1 additional discriminating question
  before opening the first branch.
- If later answers materially change the session owner, autonomy surface, or
  highest-cost mistake, revise the family read out loud instead of preserving
  the earlier classification for consistency.

## Primary-Family Rule

When more than one family is present, choose the primary family by asking:

1. What owns the session lifecycle?
2. What can cause the highest-cost architecture mistake if misclassified?
3. Which branch must be resolved first before the others make sense?

The primary family is the one that answers those questions most strongly.
Everything else is secondary pressure until proven otherwise.

## Candidate Families

### Coding

Signals:

- edits code
- runs tests or commands
- needs plan/build split or execution boundaries

### Always-On or Gateway

Signals:

- multiple channels
- external events or timers
- background tasks
- session reinjection

### Providerized Shared Core

Signals:

- multiple backends
- shared core across CLI and remote surfaces
- fallback strategy matters

### Role-Specialized Multi-Agent Pipeline

Signals:

- the domain naturally decomposes into stable specialist roles
- analyst, debate, judge, or approval stages are part of correctness
- resumable staged execution matters more than dynamic child spawning

### Lightweight or Teaching Harness

Signals:

- user needs the smallest useful loop
- architecture should stay highly legible
- risk is overbuilding too early

## Mixed-Family Priority Rules

### Always-on plus coding

Default read:

- primary family: `always-on or gateway`
- secondary pressure: `coding`

Reason:

- session ownership, routing, reinjection, and recovery must be decided before
  coding actions can safely live inside the loop

First branch:

- `always-on`

Then:

- `coding`
- `governance`

### Providerized plus always-on

Default read:

- primary family: `providerized shared-core` only if one core truly must serve
  more than one surface or fallback correctness is central
- otherwise primary family: `always-on or gateway`

Reason:

- do not let future abstraction outrank present routing pressure

First branch:

- whichever is currently blocking the design:
  - `providerized` if shared-core and fallback rules determine the whole
    runtime shape
  - `always-on` if session ownership and event routing are still undefined

### Coding plus providerized

Default read:

- primary family: `coding`
- secondary pressure: `providerized`

Reason:

- execution boundary, plan/build posture, and verification loop are usually
  more immediate than backend swaps

First branch:

- `coding`

Then:

- `governance`
- `providerized` only if backend replacement is already real

### Role-specialized pipeline plus always-on

Default read:

- primary family: `always-on or gateway` only if routing and session ownership
  are still undefined
- otherwise primary family: `role-specialized multi-agent pipeline`

Reason:

- do not let a staged analyst graph outrank unresolved control-plane ownership,
  but once routing is fixed the role graph should own the deeper runtime shape

First branch:

- `always-on` if channel/session ownership is still unclear
- otherwise `multi-agent`

### Lightweight plus anything heavier

Default read:

- primary family: the heavier family only if there is already hard evidence for
  it
- otherwise primary family: `lightweight or teaching`

Reason:

- anti-overbuild pressure should win ties in early-stage designs

First branch:

- `lightweight MVP`

Then:

- open the heavier branch only if the user’s answer proves it is necessary

## Tie-Break Questions

If two families still look equally plausible, ask one of these:

- "What must keep working when no human is actively watching it?"
- "If we get the first version wrong, is the bigger failure bad execution,
  bad routing/session ownership, or premature abstraction?"
- "Does one shared core already have to serve more than one surface on day
  one?"

## Output

State:

- primary family
- secondary pressure if any
- one heavier family that is not justified yet
- why this family wins the first-branch priority
- what branch should be opened second if the first branch resolves as expected

If the family read changes later, also state:

- previous family read
- new family read
- the evidence that forced the change
- the new first branch priority
