# When to Use Gateway

## Introduce a gateway when

- multiple channels feed one agent identity
- background events must re-enter the same harness
- session routing, identity, and runtime coordination are major concerns

## Avoid it when

- a single-session CLI or API loop already fits
- the design is still proving its core task workflow

## Preferred shape

- keep the gateway as control plane
- keep reasoning in the agent runtime

## Provenance

- openclaw
- hermes-agent
