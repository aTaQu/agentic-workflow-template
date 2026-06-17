# /plan — Turn a goal into a wave-structured plan

Authors a phased plan in `plans/`: slices work into vertical tracer-bullet criteria and groups the
independent ones into parallel waves. Front of the loop — `/plan` produces what `/next` and
`/orchestrate` consume. Adapted from Matt Pocock's `prd-to-plan` / `to-issues`, retargeted to this
repo's wave format and ADR convention.

## Step 1 — Gather the goal
Work from a goal/spec/PRD already in the conversation, or the output of a just-finished
`/grill-with-docs` session. If `$ARGUMENTS` points to a file, read it. If the goal is vague or has
unresolved design branches, **stop and run `/grill-with-docs`** (or `/grill-me`) first — don't slice
a half-grilled goal; agents amplify weak specs.

## Step 2 — Explore
If you haven't already, explore the codebase to learn the current architecture, integration layers,
and existing patterns. Use `CONTEXT.md` vocabulary throughout the plan; respect existing `docs/adr/`.

## Step 3 — Surface durable decisions
Identify high-level choices unlikely to change during implementation — route/URL shapes, schema
shape, key data models, auth approach, third-party boundaries.
- Already settled → note which `docs/adr/` entries the plan leans on.
- New, non-obvious, hard-to-reverse → **flag as an ADR candidate and record it** (`docs/adr/NNNN`,
  or run `/grill-with-docs`) *before* slicing. Durable decisions live in ADRs, not buried in the plan.

## Step 4 — Slice into vertical tracer bullets
Break the work into thin **vertical slices**, each a complete path through every layer
(schema → logic → interface → tests) that's demoable on its own. Prefer many thin slices over few
thick ones. Each slice becomes one acceptance criterion (= one task).
- Describe end-to-end behavior, not layer-by-layer steps.
- No volatile file/function names; do state durable shapes (routes, schema, model names).

## Step 5 — Group into waves by independence
Give each slice a file **Scope** (in-globs). Then:
- Slices with **disjoint scopes** and no dependency on each other → the same `### Wave N (parallel)`.
- A slice that needs another's output, or shares a scope → a **later wave** (or sequential).
- A phase with no independent group → a flat `- [ ]` list, no wave headers.
Tasks within a wave run in parallel; waves run in order (see `CONTEXT.md`). Getting independence
right here is what keeps `/orchestrate` safe.

## Step 6 — Quiz the user
Present the breakdown — phases → waves → slices, each with its Scope and the ADRs it leans on. Ask:
- Granularity right? (too coarse / too fine)
- Are the **wave groupings** correct — are the parallel slices truly independent (disjoint scopes,
  no hidden dependency)?
- Any slices to merge or split? Any durable decision still unrecorded?
Iterate until approved.

## Step 7 — Write the plan file
Create `plans/NNNN-<slug>.md` (next free numeric prefix). Template:

```markdown
# Plan: <Name>

> Goal: <one line>. Leans on ADRs: <NNNN, … or "none">.

## Phase 1 — <Title>

### Wave 1 (parallel)
- [ ] <vertical slice / acceptance criterion>
  Scope: in `<globs>`
- [ ] <...>
  Scope: in `<globs>`

### Wave 2 (sequential)
- [ ] <slice that depends on Wave 1>
  Scope: in `<globs>`

## Phase 2 — <Title>
- [ ] <flat sequential criterion — no waves needed>
```

Then tell the user: drive it with `/next` (sequential) or `/orchestrate` (parallel waves).
