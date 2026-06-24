# When to Use Memory

## Introduce memory when

- facts must survive session restart
- repeated tasks benefit from retrieval rather than re-asking the user
- the agent needs a curated long-term profile or knowledge base

## Avoid durable memory when

- session history is enough
- the remembered data has no retrieval plan
- staleness would create more risk than value

## Preferred shape

- session transcript for short horizon
- durable history for audit
- curated long-term memory for selectively retrieved facts

## Provenance

- nanobot
- hermes-agent
- Anthropic building effective agents
