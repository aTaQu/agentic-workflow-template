# Plan: Build the agentic-workflow-template

The build of this template, planned in its own wave format — real dogfood and a worked example of
the plan/wave convention. (`- [ ]` todo · `- [x]` done.)

## Phase 1 — Scaffold the workflow

### Wave 1 (parallel)

- [ ] Per-project config: `AGENTS.md` (verify hook, review rules, workflow, rules) + `CLAUDE.md` import
  Scope: in `AGENTS.md`, `CLAUDE.md`
- [ ] Vendor the 9 skills into `.claude/skills/` (8 from MP upstream + a generic `parallel-batch`) with attribution
  Scope: in `.claude/skills/**`
- [ ] Docs scaffolding: `docs/adr/README.md`, `docs/codebase-map.md` seed, a generic example plan
  Scope: in `docs/adr/README.md`, `docs/codebase-map.md`, `plans/0002-example.md`

### Wave 2 (sequential — depends on Wave 1 config)

- [ ] Port + genericize the 7 loop commands into `.claude/commands/`
  Scope: in `.claude/commands/next.md`, `.claude/commands/start.md`, `.claude/commands/task-done.md`, `.claude/commands/phase-done.md`, `.claude/commands/review.md`, `.claude/commands/next-search.md`, `.claude/commands/diff-review.md`
- [ ] Add the orchestrator commands (`/orchestrate` + the `next-wave` selection prompt)
  Scope: in `.claude/commands/orchestrate.md`, `.claude/commands/next-wave.md`

### Wave 3 (sequential)

- [ ] Finalize `README.md` (mark built, real structure)
  Scope: in `README.md`

> The waves above mark where the work is genuinely parallelizable — that's the format demo. This
> build runs them top-to-bottom in one session; a consumer with the built orchestrator could fan
> Wave 1 out with `/orchestrate` (three disjoint scopes).
