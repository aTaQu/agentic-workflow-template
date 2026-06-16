# Parallel tasks get a cheap per-task gate; thorough review at the wave gate

Each worktree task runs only a **cheap automated gate** before integrating — the project's
typecheck/lint plus its own TDD tests green — while the thorough `diff-review` runs **once on the
integrated diff** at the per-wave Review gate. Considered running full `/task-done` (static
analysis + diff-review subagent) per worktree (full symmetry with the sequential loop, but N
expensive review subagents, redundant with the gate, and blind to cross-task interactions) and
keeping `parallel-batch` as-is (leanest, but per-task non-test issues only surface at
integration). Chose the hybrid: fail fast cheaply per task, review thoroughly where cross-task
interactions are visible.
