# /review — Pre-commit code review

## Load context
Read `docs/current-task.md`. If `$ARGUMENTS` is provided, use it as a path filter on the diff.

## Diff
Run `git diff HEAD` (filtered by `$ARGUMENTS` if provided).

## Delegate review
Read `.claude/commands/diff-review.md`. Launch a subagent (subagent_type: general-purpose) using
that file's content as the prompt, substituting:
- `[TASK_CONTEXT]` with the full content of `docs/current-task.md`
- `[DIFF]` with the diff output

## Output
Print the subagent's structured report as-is.
