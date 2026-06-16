# /phase-done — Phase completion and handoff

Run when all acceptance criteria for the current phase are checked in the active plan file.

## Steps

### 1. Verify completion
Read the active plan file. Find the just-completed phase. List all criteria + status. If any
`- [ ]` remain:
```
PHASE NOT COMPLETE
Remaining:
- [ ] criterion text
```
Stop. Do not proceed until all are checked.

### 2. Full review
Run `git diff [previous phase tag or last phase-done commit]..HEAD` for the full phase diff.
Run the complete `diff-review` (read `.claude/commands/diff-review.md`) across all dimensions,
including the project's **Review rules** from `AGENTS.md`. Report issues; blockers → stop and list.

### 3. Update codebase-map.md
- Mark the phase's status complete.
- Ensure all new files/dirs added during this phase are listed under their phase section.

### 4. Commit the docs update
```
git add docs/codebase-map.md [the plan file]
git commit -m "docs(phase-N): mark phase complete, update codebase map"
```

### 5. Handoff summary
Output:
```
## Phase N complete

### What was built
[2–3 sentences]

### Acceptance criteria
- [x] all criteria

### New ADRs this phase
[docs/adr/NNNN added, or "none"]

### Files added this phase
[from codebase-map.md additions]

### Next phase
Phase N+1: [name]
First tasks:
1. [first]
2. [second]
```
Flag any dependencies or risks for the next phase.
