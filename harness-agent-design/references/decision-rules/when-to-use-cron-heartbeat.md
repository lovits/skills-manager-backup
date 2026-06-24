# When to Use Cron or Heartbeat

## Introduce cron or heartbeat when

- the agent must wake without user input
- periodic checks are part of the product
- liveness or maintenance actions need scheduling

## Avoid it when

- the only reason is "future flexibility"
- the agent has no clear event-handling or recovery design

## Preferred shape

- inject scheduled events into the same stateful loop
- record schedule and last-run artifacts explicitly

## Provenance

- openclaw heartbeat
- hermes-agent cron provider
