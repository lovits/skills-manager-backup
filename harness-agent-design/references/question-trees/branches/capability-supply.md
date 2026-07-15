# Capability Supply Branch

## Open This Branch When

- tools, MCP, skills, plugins, hooks, or commands are architectural seams
- the real question is what the harness may load, expose, or activate

## Ask Like This

Start with capability shape, not provider abstraction.

Useful options:

- `A. Small built-in toolset only`
- `B. Tools plus a narrow MCP or skill seam`
- `C. Manifest-loaded capabilities with trust boundaries`
- `D. Full plugin, hook, skill, and MCP supply chain`

Then ask:

1. Which capability types actually matter: built-in tools, MCP tools, skills,
   plugins, hooks, commands, or channel adapters?
2. What should be static versus loadable at runtime?
3. Are these capabilities user-facing, operator-facing, or internal control
   surfaces?
4. What is the trust model: local-only, project-root, signed bundle,
   marketplace, remote source, or explicit allowlist?
5. Does the system need cold manifest inspection before activation?
6. Is load planning capability-based, route-based, provider-based, or
   workspace-based?
7. Are hooks just extension points, or part of governance and policy?
8. Is MCP a narrow tool bridge, a long-lived runtime surface, or one supply
   path among several?
9. What capability must never bypass the central runtime contract?
10. Which extension seam is tempting but unjustified right now?

Do not leave this branch until load seams, trust posture, and runtime ownership
are explicit.

## Borrow From

- `OpenClaw`
- `OpenHarness`
- `claw-code`
- `hermes-agent`
- `learn-claude-code`
