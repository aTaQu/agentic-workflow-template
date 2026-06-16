# next-wave — parallel wave selection subagent prompt

Used by /orchestrate. Do not invoke directly.

Read all `plans/*.md` (precedence: numeric filename prefix ascending; within a file, top to bottom).
Find the **earliest phase containing a `### Wave N (parallel)` subsection with ≥2 unchecked `- [ ]`
tasks** — the next runnable wave. A phase with no parallel wave, or whose parallel tasks are all
checked, is not a wave target; skip it.

Also read `docs/codebase-map.md` and `docs/adr/` for context relevant to that phase.

Return ONLY:

  Phase: N — [phase name]
  Plan: [plans/ file]
  Wave: [wave heading]
  Tasks:
  - id: T1
    criterion: [exact text]
    scope_in: [globs from the task's Scope line]
    scope_out: [globs, or "everything else"]
  - id: T2
    ...
  Decisions in play:
  - ADR NNNN: [one-line summary relevant to the wave]

If no phase has an unchecked parallel wave, return exactly:

  NO PARALLEL WAVE — use the sequential loop (/next).
