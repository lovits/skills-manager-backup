# Harness Brief

## Agent Read

- one-sentence description

## Family Read

- primary family
- secondary pressure if any
- one heavier family explicitly rejected for now

## Situation Read

- who uses it
- in what moment
- how much action it is expected to take
- success signal
- failure signal

## Recommended Runtime Shape

- current best-fit runtime shape
- why smaller is not enough
- why heavier is not justified yet

## Runtime Story

- the minimum loop that explains how it works
- any justified state transition such as `plan -> build` or
  `event -> session reinjection`

## Activated Branch Order

List only the branches that actually mattered, in the order they were
resolved.

For each branch:

- what pressure activated it
- key choice made
- what was explicitly deferred

## Checked but Closed

- branch -> why it was checked
- why it stayed closed

## Borrowed Methods

- subsystem -> borrowed project method -> why it fits
- subsystem -> rejected heavier method -> why not yet

## Do Not Build Yet

- list the tempting complexity to avoid for v1
- name any heavier project shape that is not justified yet

## Next Build Order

1. smallest useful version
2. next justified capability
3. later expansions if pressure appears
