# /task-done — Task completion checklist

**Task:** $ARGUMENTS

## Steps

### 1. Load state
Read `docs/current-task.md` only. Phase, criterion, scope, and relevant ADRs are all there.

### 2. Verify
Run the project's **Verify** commands (the `## Verify` section of `AGENTS.md` — typecheck / lint /
tests). Any failure → `NEEDS FIXES` + the failing output. Stop. Fix, then rerun before continuing.

### 3. Delegate diff review
Run `git diff HEAD`.
Read `.claude/commands/diff-review.md`. Launch a subagent (subagent_type: general-purpose) using
that file's content as the prompt, substituting:
- `[TASK_CONTEXT]` with the full content of `docs/current-task.md`
- `[DIFF]` with the output of `git diff HEAD`

### 4. Act on verdict
NEEDS FIXES → output the numbered list, stop. Fix, then rerun `/task-done`.
READY TO COMMIT → continue.

### 5. Update plan
Mark the criterion `- [x]` in the plan file that owns it (recorded in `docs/current-task.md`).

### 6. Update codebase map
If the diff added new files: add them to `docs/codebase-map.md` under the right phase section.

### 7. Commit
```
git add [specific files only]
git commit -m "type(phase-N): $ARGUMENTS"
```

### 8. New decision?
If the review flagged an **ADR candidate**: add `docs/adr/NNNN-slug.md` now (next free number;
title + 1–3 sentences — see `.claude/skills/grill-with-docs/ADR-FORMAT.md`).

### 9. What's next
List remaining unchecked criteria for the current phase.
All checked → "Phase complete — run `/phase-done`." Stop; do NOT chain to /next.
Not done → continue to step 10.

### 10. Update current-task.md
Overwrite with:
```markdown
# Current Task
## Status
Task "[title]" complete. Run /next for the next task.
```

### 11. Chain to /next
Unless the phase is complete (step 9), follow `.claude/commands/next.md` now to auto-select and
write the next task into `docs/current-task.md`. Then stop — the user starts a fresh session and
runs `/start`.
