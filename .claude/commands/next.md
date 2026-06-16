# /next — Pick and execute the next task

## Step 1 — Launch search subagent

Read `.claude/commands/next-search.md` and pass its content as the prompt to a subagent
(subagent_type: Explore).

Do not read `plans/`, `docs/codebase-map.md`, or `docs/adr/` yourself.

Pick the next single task from the subagent output.

**Default: a task = one full acceptance criterion.** Ship it whole.

**Split only when the criterion is genuinely large**, by one of these objective tests:
- Spans independent integration surfaces (e.g. two external providers, two unrelated UI flows)
- Would touch more than ~8 files or ~400 LOC of net change
- Crosses a hard layering boundary that needs separate verification (e.g. a schema migration that
  must deploy before the code that uses it)

**Bundle mechanical repetition.** If N nearly-identical changes are needed (e.g. the same key
added to three locale files; three handlers differing only by a constant), that is **one** task.

**No skeleton-only tasks.** A new function/service must ship with at least one caller in the same
task. A new route must ship wired to something observable. If the only deliverable is "scaffolding
nothing calls yet," fold it into the task that uses it.

**Negative examples — do NOT split this way:**
- ❌ `add URL builder` → `add redirect route` → `add signature check` → `wire callback` → `send email`
  ✅ One task: `Payment redirect + callback end-to-end`
- ❌ `add job for state A` → `add job for state B` → `add job for state C` (identical but for a constant)
  ✅ One task: `Scheduler handles all three states`

**Self-check before writing current-task.md:** glance at `git log --oneline -10`. If recent commits
are mostly <100 LOC across <3 files, bias one notch coarser this round.

## Step 2 — Write current-task.md

Using the subagent output, overwrite `docs/current-task.md`:

```markdown
# Current Task

## Phase N — [Phase name]
**Task:** [title]
**Criterion:** [exact text of the acceptance criterion]
**Plan:** [which plans/ file owns it]

## What to build
[2–4 sentences. Specific: file paths, names, config keys.]

## Acceptance criteria
- [ ] [specific, testable criterion]

## Scope
**In:** [files/dirs that may be touched]
**Out:** [must not touch]

## Decisions in play
[Only the ADRs relevant to this task — 1 line each, by number. Skip the rest.]

## Rules
- Implement only this task. Stop when the criteria above are met.
- Do not touch out-of-scope files.
- For non-trivial behavior, use /tdd (red → green → refactor).
- Blocker → stop and report. Do not improvise.
- When done: run /task-done [task title]
```

## Step 3 — Stop

After writing `docs/current-task.md`, output:

```
Task selected: [title]
Start a fresh session and run /start to implement it.
```

Do not implement. Do not read any more files. The session ends here.
