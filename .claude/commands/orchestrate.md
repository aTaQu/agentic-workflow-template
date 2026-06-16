# /orchestrate — Run the next parallel wave (the speed path)

Implements a wave of independent, pre-specced tasks concurrently in git worktrees, then stops at a
review gate and (on approval) advances to the next wave. Built on the `parallel-batch` skill — this
command is the plan-fed front end + the per-wave gate loop. See ADRs 0003–0005, 0009, 0010.

## Step 1 — Select the wave
Read `.claude/commands/next-wave.md` and pass its content as the prompt to a subagent
(subagent_type: Explore).
- `NO PARALLEL WAVE` → tell the user to use `/next` (sequential). Stop.
- Otherwise take the returned wave: its tasks, each with `scope_in` / `scope_out`.

## Step 2 — Validate independence (the safety gate)
Check the declared scopes pairwise: **no two tasks' `scope_in` globs may overlap**, and no task's
`scope_in` may fall inside another's `scope_out`.
- Overlap → STOP. Report the conflicting tasks + globs; ask the user to re-scope in the plan or drop
  a task from the wave. Do not fan out.
- Clean → continue.

## Step 3 — Worktrees
Stash or commit any dirty files on the default branch first. Then, per task:
```
git worktree add .worktrees/<task-id> -b feat/<task-id> <default-branch>
```

## Step 4 — Fan out (the win)
One `Workflow` invocation with one `agent()` per task. Each brief contains:
- The task's **criterion** + locked **Scope** (in/out) — the agent must not touch anything outside `scope_in`.
- Project conventions from `AGENTS.md`, the relevant ADRs, and file refs.
- **TDD discipline** (`/tdd`): decompose into 2–5 vertical-slice behaviors; RED → GREEN → commit per
  slice; test public interfaces; refactor only when green. Pure-styling tasks return `testStatus: visual-only`.
- **Per-task gate:** before returning, run the project's **Verify** commands (`AGENTS.md`) inside the
  worktree; tests must be green.
- **Schema-forced return:** `{task, branch, filesChanged, testStatus, tddCycles, summary, notes}`.

## Step 5 — Verify
Run each branch's tests in the main worktree via `git checkout --detach feat/<task-id>` — **not** by
symlinking dependencies into the worktree (a symlinked dependency tree resolves to the default
branch's source and produces false passes across every branch). For frameworks whose attributes the
HTML parser silently drops (Alpine `@click`, Vue `:class`, …), assert against the raw response body.

## Step 6 — Integrate
- Create one `integration-<phase>-w<N>` branch off the default branch.
- Merge each feature branch **smallest-first** to minimize cascading conflicts.
- Open **one squash PR** for the whole wave (not N PRs).

## Step 7 — Review gate (STOP)
Run the full `diff-review` (`.claude/commands/diff-review.md`) on the integrated diff, then hand the
squash PR to the user. **Stop and wait for approval.** Do not start the next wave.

## Step 8 — On approval: next wave
When the user approves and the PR is merged:
- Mark the wave's criteria `- [x]` in the plan; update `docs/codebase-map.md`; add any flagged ADRs.
- Clean up: `git worktree remove .worktrees/<each>`; `git branch -D feat/<each> integration-<phase>-w<N>`.
- Return to Step 1 for the next wave. When `next-wave` returns `NO PARALLEL WAVE`, the phase's
  parallel work is done — report and stop.
