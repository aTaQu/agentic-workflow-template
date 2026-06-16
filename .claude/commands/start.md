# /start — Implement the current task

## Step 1 — Load
Read `docs/current-task.md`. This is the only file needed to begin.

## Step 2 — Implement
Build exactly what's described. Scope is declared in the file — do not touch out-of-scope files.

**For non-trivial behavior, use `/tdd`** — decompose into vertical-slice behaviors and run
red → green → refactor per slice, testing public interfaces (rendered output, responses, exported
functions), not internals.

No confirmation needed unless a blocker appears.

**Blocker format:**
```
BLOCKER: [description]
Options:
1. [option A — trade-off]
2. [option B — trade-off]
```
Then stop. Do not improvise.

## Step 3 — Complete
When all criteria in `docs/current-task.md` are met, run `/task-done [task title]` automatically.
