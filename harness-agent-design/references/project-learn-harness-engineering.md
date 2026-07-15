# Learn Harness Engineering Notes

GitHub: [shareAI-lab/learn-claude-code](https://github.com/shareAI-lab/learn-claude-code)

## Why This Project Matters

learn-claude-code is a strong source not because it is a production harness to
copy, but because it is a mechanism-first curriculum for deciding when a
harness should grow.

Its deepest lesson is:

- the core loop stays the same
- the harness grows around the loop one justified mechanism at a time
- each added mechanism should answer a clear pressure
- the final system is still one loop, not a replacement for the loop

Use this project as a source for question order, scope control, and teaching
posture.

Its architecture is distributed across:

- `README.md`
- `s01_agent_loop/` through `s20_comprehensive/`
- `agents/s01_agent_loop.py` through `agents/s12_worktree_task_isolation.py`
- `agents/s_full.py`
- `s20_comprehensive/README.md`

Read this project for questions like:

- what mechanism should be added next, and which one should wait
- how to explain harness architecture without collapsing into platform jargon
- which advanced mechanism is truly justified by current pressure
- how several mechanisms can still compose back into one stable loop

## Subsystem Methods

### Mechanism-first growth on top of an invariant loop

Borrow:

- one stable agent loop as the invariant core
- every new mechanism framed as something that wraps, feeds, or constrains that
  loop
- "many mechanisms, one loop" as the governing design rule
- pressure-based mechanism addition instead of template-driven accumulation

Use when:

- the user needs architecture help without overbuilding
- the design conversation risks jumping to advanced subsystems too early
- you want the interview to converge by pressure rather than by checklist

Avoid when:

- the user already needs exact production runtime details, not conceptual
  sequencing

Deep lesson:

- learn-claude-code teaches that the loop is constant
- the harness is what changes around it

### Canonical escalation order

Borrow the teaching sequence as a pressure ladder:

1. loop
2. tools
3. permissions
4. hooks
5. planning / todo
6. subagents for context isolation
7. skills for on-demand knowledge
8. compaction
9. memory
10. runtime prompt assembly
11. error recovery
12. task system
13. background tasks
14. cron scheduling
15. agent teams
16. team protocols
17. autonomous claiming
18. worktree isolation
19. MCP/plugin capability expansion
20. capstone recomposition

Use when:

- the user has a vague agent idea and needs the next justified design decision
- you need to explain why some mechanisms are deferred
- you want a principled "start simple, then add pressure-specific machinery"

Avoid when:

- the product already has clear always-on, compliance, or service-runtime
  requirements that force several mechanisms from day one

### Teaching decomposition by mechanism

Borrow:

- each lesson isolates one mechanism and one reason for adding it
- code, explanation, and capstone integration are all aligned
- architectural complexity explained by placement in the loop, not by category
  sprawl
- a design style that distinguishes base primitive from surrounding support

Use when:

- the skill should behave like a senior engineer teaching architecture
- the user is new to harness design and needs a clean conceptual map
- you need to explain a complicated harness without sounding mystical

### Pressure-gated subagents, teams, and autonomy

Borrow:

- clear distinction between one-shot subagents, persistent teammates,
  team protocols, autonomous claim loops, and worktree isolation
- the rule that each step solves a different pressure: context isolation,
  collaboration, communication reliability, self-organization, filesystem
  isolation
- no premature collapse of these concepts into one vague "multi-agent" bucket

Use when:

- the user says "I want multi-agent" without clear semantics
- you need to interview for the actual pressure behind delegation
- the design risks mixing helper runs, routed personas, and persistent teams

Deep lesson:

- multi-agent is not one mechanism
- it is several mechanisms that should activate separately

### Capstone recomposition

Borrow:

- the final architecture view that puts hooks, permissions, memory, MCP,
  background work, cron, teams, and worktrees back onto one loop
- a disciplined map of where each mechanism sits relative to the LLM call and
  tool cycle
- architectural narration based on loop phases rather than on a file tree

Use when:

- the user has enough context and now needs the whole runtime picture
- you want to explain how several justified mechanisms coexist without chaos
- the design is approaching comprehensive-harness territory

Avoid when:

- the system is still clearly a lightweight MVP

### Mechanism-specific branch prompts

Borrow the question style itself:

- ask for the next pressure, not the next trendy feature
- ask what the mechanism protects, enables, or isolates
- ask what must be true before a more advanced mechanism is warranted
- ask what can stay out of v1

Use when:

- the skill should feel like a real architecture interview
- you want branching questions to be principled instead of arbitrary

## Question Angles This Project Justifies

- Which mechanism is actually next?
- What problem is the user trying to solve that the current loop cannot yet
  handle?
- Is this memory pressure, background-work pressure, delegation pressure,
  always-on pressure, or provider pressure?
- What can stay outside v1 without breaking the product?
- If several mechanisms are justified, where do they attach to the loop?

## Design Warnings

- Do not treat the curriculum order as a rigid production checklist.
- Do not import every mechanism because it appears in the capstone.
- Do not mistake a teaching implementation for an exact production contract.
- Do not let advanced concepts replace the basic question: what pressure makes
  this mechanism necessary now?
