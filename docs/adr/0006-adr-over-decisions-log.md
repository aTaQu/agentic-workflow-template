# Decisions are recorded one-file-per-ADR in docs/adr/

The project records decisions as individual ADR files under `docs/adr/` (the Matt Pocock
convention) rather than as `D-###` entries in a single `docs/DECISIONS.md`. One file per decision
means N parallel worktree agents never conflict on the decision log, which **retires
`parallel-batch`'s `D-###` pre-assignment hack**. `grill-with-docs` and
`improve-codebase-architecture` then run unmodified. In exchange, the ported sequential loop's
`task-done` / `diff-review` steps are re-pointed from "DECISIONS.md candidate" to "new ADR,"
breaking the prior `D-###` habit. `CONTEXT.md` is kept (both conventions already shared it).
