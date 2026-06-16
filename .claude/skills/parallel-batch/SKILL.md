---
name: parallel-batch
description: Orchestrate 2-6 independent feature implementations in parallel using worktrees, sub-agents, and a single integration-branch squash-merge. Use when the user submits multiple independent threads in one message (numbered list, "and also", "all four"), explicitly says "do these in parallel" / "fan out" / "parallelize", references the parallel-batch pattern by name, or has 3+ unrelated features to ship together. Do NOT trigger for a single feature however large, dependent features where B builds on A, high-uncertainty design space, or sweeping refactors.
---

# Parallel batch

Run N independent features in parallel via grill → scout → worktree fan-out → schema-validated agent
implementations → one integration squash PR → iterate → cleanup. Features must be independent (touch
mostly disjoint files) and individually lockable. If they aren't, go serial.

> The general parallel-implementation engine. The `/orchestrate` command is the plan-fed front end
> that feeds this engine pre-specced tasks from a wave; you can also invoke this skill directly on
> ad-hoc independent features. **"Verify"** below means the project's verify/test commands declared
> in the `## Verify` section of `AGENTS.md`.

### Phase 1 — Detect & grill (serial, interactive)
- **For each feature, invoke `/grill-with-docs`** — it challenges the plan against the domain
  glossary (`CONTEXT.md`) and the ADR log, sharpens terminology, resolves every branch, and records
  decisions as **new files in `docs/adr/`**. Because each decision is its own ADR file, N parallel
  features never conflict on the decision log.
- Lock each feature into a **1-3 sentence scope statement** that pastes into briefs verbatim.
- Fallback: no glossary/ADR convention → `/grill-me` (lighter); trivial scope → ad-hoc
  `AskUserQuestion`. Never skip grilling — agents amplify whatever specs you hand them.

### Phase 2 — Scout (parallel, read-only)
- Fan out **N+1 `Explore` agents in one message** — one per feature + one for project conventions /
  infra (the Verify command, ADR format, devcontainer specifics).
- Each returns file:line refs + 3-5 sharp follow-up questions. Run one more grilling pass before
  locking briefs.

### Phase 3 — Worktrees (inline, cheap)
- `git worktree add .worktrees/<feat> -b feat/<feat> <default-branch>` per feature. Stash any
  uncommitted files on the default branch first.

### Phase 4 — Parallel implementation with enforced TDD (the win)
- One `Workflow` invocation with N parallel `agent()` calls.
- **Each agent follows `/tdd` within its scope** (canonical doc: `.claude/skills/tdd/SKILL.md`):
  - Decompose the locked scope into **2-5 vertical-slice behaviors** before writing code.
  - For each: **RED test → minimal GREEN impl → commit the cycle → next.** One test, one impl.
  - **No horizontal slicing** (all tests first, then all code). >1 test in a single commit with no
    intervening impl is a smell — re-run.
  - Refactor only **after all targeted tests are green**; never while RED.
  - Tests exercise **public interfaces** (rendered output, responses, exported functions), not internals.
  - The agent runs the project's **Verify** commands in its worktree so it can actually go
    red→green. If the toolchain can't run in a worktree, it writes tests with extra care and
    Phase 5 confirms each cycle.
- **Pure-styling exception**: features with no behavior contract skip TDD and return
  `testStatus: 'visual-only'`. Flag these upfront; don't shoehorn TDD into pixel-only changes.
- **Schema-forced return**: `{feature, branch, prUrl, filesChanged, testStatus, tddCycles, summary, notes}`.
  `tddCycles` = list of `{behavior, redCommit, greenCommit, observation}`.
- Each brief includes: locked scope statement, relevant ADRs, file:line refs from scouting, project
  conventions, the pitfalls below, and the TDD rules above.

### Phase 5 — Verify (serial, careful — read this twice)
- **Run tests via `git checkout --detach feat/<X>` in the main worktree**, NOT by symlinking the
  dependency tree into the feature worktree. A symlinked dependency tree resolves to the default
  branch's source and produces **false-pass results across every branch** — the silent-failure trap
  that bites hardest.
- For UI frameworks with non-XML-name attributes (Alpine `@click.prevent`, Vue `:class`, `x-data`,
  etc.), **assert against the raw response body** (substring match after entity-decoding), since HTML
  parsers silently drop them.

### Phase 6 — Integrate (serial)
- One `integration-<N>-features` branch off the default branch. Merge each feature **smallest-first**
  to minimize cascading conflicts. Open **one squash-merge PR** for combined review — not N PRs.

### Phase 7 — User iterates (serial)
- For UI work, **tests-green is the START of review, not the end**. Budget 3-5 iterative commits on
  the integration branch; the visual is the real acceptance test. Clear caches between branch switches.

### Phase 8 — Merge + cleanup
```
gh pr merge <integration-pr> --squash --delete-branch
git pull --ff-only origin <default-branch>
git worktree remove .worktrees/<each>
git branch -D feat/<each> integration-<N>-features
git remote prune origin
```

## When NOT to use
- **Single feature**, however large → TDD or direct implementation.
- **Dependent features** (B builds on A) → serial; parallelism produces broken merges.
- **High-uncertainty design space** → grill more before parallelizing.
- **Pure refactors / sweeping renames** → one agent with `isolation: "worktree"`, not N parallel.
