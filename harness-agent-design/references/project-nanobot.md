# Nanobot Notes

## Why this project matters

This project is a strong reference for minimal harness engineering with clear
separation between loop, runner, and context building.

## Reusable patterns

- small readable loop
- orchestrator versus executor separation
- layered memory with file-backed artifacts
- JSONL session durability

## When to borrow from it

- MVP agents
- solo-agent loops
- agents where readability and iteration speed matter
- first versions that need a working harness before a grand architecture

## Question mother-types from this project

- What is the smallest loop that fully explains the job?
- Can this remain a single readable harness for v1?
- Which abstractions can wait until real pressure appears?
- What state can stay file-backed and simple?

## Design warnings

- do not add providers or multi-agent roles until the small loop proves
  insufficient
