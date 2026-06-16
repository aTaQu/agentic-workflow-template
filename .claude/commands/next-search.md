# next-search — plan search subagent prompt

Used by /next. Do not invoke directly.

Read these in parallel:

- **All `plans/*.md`** — find the earliest phase, across all plan files, with at least one unchecked
  `- [ ]` criterion. **Precedence:** plan files are ordered by numeric filename prefix ascending
  (`0001-…` before `0002-…`); within a file, top to bottom. **Ignore `### Wave N` headers** — walk
  criteria in document order (waves are for the orchestrator; `/next` is sequential).
- `docs/codebase-map.md` — extract entries relevant to that criterion's phase.
- `docs/adr/` — extract ADRs relevant to that criterion.

Return ONLY this structure, no commentary:

  Phase: N — [phase name]
  Plan: [plans/ file]
  Criterion: [exact criterion text]
  Scope note: [default "whole criterion". Propose a split only if the criterion meets one of:
    spans independent integration surfaces; >~8 files / >~400 LOC; or a layering boundary needing
    separate verification. State which test triggered it and what the first slice is. Do NOT split
    mechanical repetition (N near-identical changes); do NOT propose skeleton-only slices.]
  Codebase context:
  - [relevant codebase-map entry]
  Decisions in play:
  - ADR NNNN: [one-line summary]
