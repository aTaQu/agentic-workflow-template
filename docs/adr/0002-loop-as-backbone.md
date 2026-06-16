# The sequential task-loop is the backbone, ported with review

The workflow is built around the sodyba task-loop (`/next` → `/start` → `/task-done` →
`/phase-done`) as its spine, rather than shipping the MP skills as a loose collection. The chosen
skills correspond to the loop's phases (grill = design, tdd = inside `/start`, handoff = session
boundary, prototype = pre-design exploration). The loop is **ported with critical review, not
copied verbatim** — its Symfony-specific coupling (PHPStan step, `diff-review` rules, hardcoded
plan filenames) is extracted into per-project hooks. Considered skills-only (leaner, but loses
the task-sizing doctrine and the per-task review/commit rhythm that make the loop productive).
How tightly the skills are wired into the commands is a separate, later decision.
