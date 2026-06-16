# Skills are soft-wired into the loop via nudges

The vendored skills are referenced from the loop's commands as **one-line nudges** at the
relevant phase (e.g. `/start` suggests `/tdd` for non-trivial behavior; the design step points at
`/grill-with-docs`; a context-budget note suggests `/handoff`) while remaining independently
invocable. Considered hard-wiring (commands invoke skills as steps — maximum enforcement, but
rigid and risks a command and a skill both trying to steer) and full separation (orthogonal —
cleanest, but the discipline isn't self-reinforcing and tends to slip). Chose nudges to make the
workflow self-reinforcing without forcing it.
