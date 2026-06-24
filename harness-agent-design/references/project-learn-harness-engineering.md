# Learn Harness Engineering Notes

## Why this project matters

This project is a teaching-oriented reference that breaks harness engineering
into composable concepts rather than one monolithic architecture.

## Reusable patterns

- tool loop as the base primitive
- context compaction as a first-class harness concern
- subagents for context isolation
- task systems for long work
- background tasks for async behavior
- teams and protocols for coordinated agents
- worktree isolation for risky coding work
- skill loading for just-in-time specialization

## When to borrow from it

- educational designs
- coding agents
- systems that need a principled decomposition of harness features

## Design warnings

- these concepts are building blocks; do not load all of them into the first
  version of an agent
