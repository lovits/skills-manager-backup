# Lightweight MVP Branch

## Open This Branch When

- the right answer may be to stay small and legible

## Ask Like This

Keep asking what can be left out.

Useful options:

- `A. One simple loop`
- `B. One loop plus tool use`
- `C. One loop plus one justified support mechanism`
- `D. One readable loop plus a small runner/context split`

Then ask:

1. What is the minimum useful loop?
2. Is one loop enough, or does readability already justify a small split
   between orchestration, execution, and context assembly?
3. Which proposed subsystem can be deferred?
4. What future pressure would justify memory, gateway, delegation, or provider
   abstraction later?
5. What would make this design too clever for v1?
6. Which state can stay file-backed and local for now?

Do not leave this branch until the minimum loop, biggest deferral, and future
upgrade trigger are explicit.

## Borrow From

- `nanobot`
- `learn-claude-code`
