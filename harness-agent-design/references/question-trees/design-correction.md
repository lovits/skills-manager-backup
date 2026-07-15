# Design Correction Intake

## Goal

Use this file when the user already has a harness shape and wants help finding
what is wrong, overbuilt, missing, or misclassified.

This is not a blank-sheet interview. Start from the current design, localize
the wrong turn, then reopen only the branches needed to fix it.

## Open This Mode When

- the user says the agent is already partly designed
- the current design feels wrong, too heavy, too weak, or internally
  inconsistent
- the user wants to keep some existing decisions while correcting others

## Ask Like This

Map the current design before proposing changes.

Useful options:

- `A. Small mismatch, keep most of the current design`
- `B. One subsystem is wrong and needs replacement`
- `C. The current family read is wrong`
- `D. The architecture needs a major reshape`

Then ask:

1. What is already decided: loop, memory, tools, gateway, multi-agent,
   provider layer, background work, or governance?
2. What symptom triggered the redesign: overbuild, missing capability, unstable
   state, memory pollution, unsafe tools, routing confusion, or poor recovery?
3. Which parts should be preserved if possible?
4. Which part now looks most suspicious: family read, branch choice, state
   model, capability surface, or autonomy level?
5. What invariant is currently broken: session ownership, memory scope,
   execution boundary, provider contract, task lifecycle, or approval posture?
6. Is the current design too heavy, too weak, or simply misarranged?
7. What is the smallest subsystem-level change that could fix it?
8. Which tempting bigger redesign should be resisted unless the smaller fix
   fails?

## What To Produce

Before proposing a new architecture, state:

- current design read
- likely wrong turn
- what survives unchanged
- what must be changed first
- what branch should be reopened to validate the correction
