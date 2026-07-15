# TradingAgents Notes

GitHub: [TauricResearch/TradingAgents](https://github.com/TauricResearch/TradingAgents)

## Why This Project Matters

TradingAgents is a strong source for harnesses where "multi-agent" does not
mean dynamic child spawning, but a stable domain pipeline made of specialist
roles, debate loops, judge stages, structured outputs, checkpoint resume, and
data-vendor discipline.

The deeper lesson is that some agent systems are better designed as:

- a fixed specialist graph
- role-specific tool surfaces
- explicit state passed between stages
- resumable long-running analysis
- domain memory that reflects on past outcomes

For this skill, TradingAgents should not be reduced to "lots of analyst
personas." It is most useful as a method source for role-stable
decision/research pipelines.

Its codegraph clusters around:

- `tradingagents/graph/trading_graph.py`
- `tradingagents/graph/setup.py`
- `tradingagents/graph/analyst_execution.py`
- `tradingagents/graph/checkpointer.py`
- `tradingagents/agents/utils/memory.py`
- `tradingagents/llm_clients/factory.py`
- `tradingagents/llm_clients/capabilities.py`
- `tradingagents/agents/utils/agent_utils.py`
- `tradingagents/dataflows/market_data_validator.py`
- `README.md`

Read TradingAgents as a method source for at least these subsystem questions:

- when a problem should be a role-stable pipeline instead of one owner loop
- when one graph should use different reasoning tiers for different stages
- how specialist stages should each see different tools and outputs
- when debate and judge loops are more appropriate than free-form delegation
- how checkpoint resume should work for a long, staged analysis
- how domain memory should reflect on realized outcomes rather than store raw
  user preference or undifferentiated transcript scraps
- how provider capability quirks should be encoded declaratively
- how structured output becomes a runtime contract instead of a prompt wish
- how external data-vendor correctness becomes a harness concern and a stage
  gate

## Subsystem Methods

### Role-stable graph instead of free-form delegation

Borrow:

- a fixed role topology rather than ad hoc child spawning
- analyst stages that run in a deliberate sequence
- explicit downstream stages for bull/bear debate, trader synthesis, risk
  debate, and portfolio decision
- multi-agent behavior implemented as a workflow graph, not a bag of prompts

Use when:

- the domain naturally decomposes into persistent specialist roles
- review, counter-argument, or approval stages are part of correctness
- the system should be predictable and inspectable stage by stage

Avoid when:

- one strong owner loop can already do the work
- the "specialists" are merely decorative prompt variants

Deep lesson:

- TradingAgents uses many agents because the domain has stable roles
- not because multi-agent sounds impressive

### Analyst plan and sequential specialist execution

Borrow:

- an explicit analyst execution plan
- two reasoning tiers where deeper models own high-stakes synthesis while
  lighter models can own narrower or cheaper stages
- deterministic ordering of role execution
- analyst-specific tool nodes and report keys
- role timing/progress tracking separate from model output

Use when:

- several specialists each need their own bounded tool surface
- the runtime should know which stage is running and what output it owes
- the user must be able to inspect stage-level progress

Avoid when:

- specialists are interchangeable and do not justify separate execution nodes

### Tiered reasoning topology

Borrow:

- separate quick-thinking and deep-thinking model clients created once at the
  graph boundary
- stage-level assignment of reasoning depth instead of one blanket model choice
- provider-specific reasoning knobs forwarded in one place before stages are
  built
- the idea that model topology is part of runtime design, not a late tuning
  knob

Use when:

- early stages can be cheaper or faster than final synthesis
- the runtime needs different reasoning budgets for planning, debate, and
  decision
- provider-specific thinking controls affect correctness or cost

Avoid when:

- the full graph is short enough that one model tier is operationally simpler

Deep lesson:

- TradingAgents does not just have many roles
- it gives different parts of the graph different reasoning budgets on purpose

### Specialist tool surfaces and verified data gates

Borrow:

- analyst-specific `ToolNode` groupings rather than one shared tool bag
- deterministic tool surfaces such as market, news, social, and fundamentals
- validated snapshots and safe symbol normalization as explicit gates ahead of
  reasoning
- the idea that some stages owe verified domain context before they are allowed
  to reason freely

Use when:

- different specialist stages should see different external capabilities
- upstream data quality can poison downstream reasoning
- the runtime should know which stage is allowed to touch which data source

Avoid when:

- role separation exists only in prompts while all roles still see the same
  ungoverned tool surface

### Debate loops and judge stages

Borrow:

- bull versus bear loops as explicit adversarial reasoning stages
- risk debate as a separate subsystem from core research synthesis
- conditional logic that decides whether debate continues or hands off
- manager/judge stages that convert several reports into a decision

Use when:

- the design needs structured challenge, not just one synthesis step
- conflicting positions should be explicit runtime states
- final approval should happen after domain-specific adversarial review

Avoid when:

- the extra debate loop only adds latency and role theater

Deep lesson:

- TradingAgents distinguishes specialist analysis, adversarial challenge, and
  final decision
- that is a reusable harness shape beyond trading

### Graph state as the runtime contract

Borrow:

- one typed state object carried through the graph
- explicit slots for reports, debate histories, counts, instrument context, and
  prior context
- runtime wiring where each node knows what state it reads and writes
- the idea that multi-agent memory inside a run should be explicit state, not
  hidden transcript magic

Use when:

- a staged agent pipeline needs inspectable intermediate artifacts
- each role should operate on shared but structured state
- you want recovery and debugging at the state-transition level

Avoid when:

- the whole workflow is one short conversational exchange

### Checkpoint resume for staged runs

Borrow:

- per-entity SQLite checkpoints rather than one global in-memory flow
- deterministic thread IDs derived from domain identity
- checkpoint stores isolated by entity so concurrent runs do not trample each
  other
- step-based resume so a failed run restarts from the last successful node
- explicit separation between checkpoint state and final completion cleanup

Use when:

- runs are expensive, long, or crash-prone enough that redoing everything is
  wasteful
- there is a stable domain key such as ticker/date, case ID, or research job
  ID
- staged graph execution should survive interruption

Avoid when:

- restarting from scratch is cheap and simpler

This is a stronger method than generic "save progress." It gives the skill a
way to ask whether resume identity should attach to a stable domain key such as
`ticker + date`, case ID, or workflow job ID.

### Domain memory as reflection on outcomes

Borrow:

- append-only decision log
- pending entry first, reflection later after real outcome is known
- an explicit two-phase memory lifecycle where decision capture happens during
  the run and reflection is written only after the outcome is measurable
- same-entity memory plus cross-entity lessons
- domain memory used to inject lessons into future runs, not just archive text

Use when:

- the system should learn from realized downstream results
- past decisions should influence later judgment in a bounded way
- memory is domain reflection, not generic personalization

Avoid when:

- there is no trustworthy feedback signal to write back into memory

Deep lesson:

- TradingAgents memory is not a generic user-memory layer
- it is a performance-reflection subsystem

### Structured-output contract and graceful fallback

Borrow:

- typed result schemas per specialist or decision stage
- render functions that convert structured records into stable downstream text
- graceful fallback to free text when the provider cannot honor structured
  output
- validation/coercion logic that treats malformed structured output as a known
  runtime risk
- the idea that output shape compatibility belongs to harness design, not only
  to prompt tuning

Use when:

- downstream stages depend on stable fields rather than informal prose
- providers disagree about tool choice, JSON mode, or schema behavior
- the system needs deterministic artifacts even when one provider path degrades

Avoid when:

- every stage is purely advisory and no later node depends on field-level
  stability

Deep lesson:

- TradingAgents treats structured output as a contract between stages
- not as a best-effort formatting request

### Declarative provider capability table

Borrow:

- provider/model client factory with lazy imports
- declarative capability table for tool choice, JSON mode, JSON schema, and
  reasoning-specific quirks
- compatibility policy encoded once instead of repeated `if model == ...`
  ladders
- provider-specific thinking/reasoning knobs forwarded in one place

Use when:

- structured output behavior changes by model or provider
- provider differences are a long-term maintenance pressure
- adding support for new models should not require editing every client path

Avoid when:

- the product only supports one backend with stable semantics

This is one of TradingAgents' strongest reusable methods because it turns
providerization into a capability-table problem instead of client spaghetti.

### Data vendor correctness as harness policy

Borrow:

- domain tools grouped by data source and analyst role
- verified market snapshots and validator layers as first-class tooling
- stale-data guards and vendor-routing tests as correctness fixtures
- symbol normalization and safe identifier handling
- vendor fallback and hygiene treated as part of the runtime boundary

Use when:

- external data quality can invalidate the agent's reasoning
- the assistant is only as good as its input feeds
- different stages depend on different vendor slices

Avoid when:

- the system is not grounded in external domain data

Deep lesson:

- in systems like TradingAgents, data-vendor correctness is harness design
- not just "backend implementation detail"

## Question Angles This Project Justifies

- Is this really dynamic delegation, or a stable role pipeline?
- Which stages need quick reasoning and which need deeper synthesis?
- Which roles are permanently justified by the domain, and which are not?
- Do you need adversarial debate and judge stages, or just one synthesizer?
- Should each specialist own a different tool surface or the same one?
- What state must be explicit between stages?
- Is restart-resume worth checkpointing at the graph-step level?
- Is memory generic chat memory, or outcome-based reflective memory?
- Does the system need stable typed outputs between stages?
- Do model/provider quirks belong in one capability registry?
- What external data feed errors are severe enough to shape the harness?

## Design Warnings

- Do not use a role graph if one owner loop can already carry the task.
- Do not call a fixed pipeline "multi-agent intelligence" if it is really just
  a staged workflow.
- Do not add debate loops when the domain does not justify structured
  disagreement.
- Do not ignore provider/data-vendor quirks in a structured-output,
  domain-grounded system.
