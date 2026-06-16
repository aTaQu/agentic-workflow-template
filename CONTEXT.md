# Agentic Workflow

The shared vocabulary of this template's workflow — the words the commands, skills, plans, and
ADRs all use. These are terms specific to *how work moves through this repo*, not general
programming concepts.

## Language

### Workflow units

**Plan**:
A phased list of acceptance criteria defining a body of work. Lives in `plans/`.
_Avoid_: backlog, roadmap, spec.

**Phase**:
A named group of criteria within a Plan, closed as a unit via `/phase-done`.
_Avoid_: milestone, sprint.

**Criterion** (acceptance criterion):
A single, testable statement of done in a Plan. The default unit of work.
_Avoid_: requirement, story, ticket.

**Task**:
The unit of work taken on in one session — by default exactly one Criterion, split only by the
objective tests in `/next` (independent integration surfaces, >~8 files / >~400 LOC, or a hard
layering boundary).
_Avoid_: subtask, ticket, issue.

### The two loops

**Sequential loop**:
The one-Task-at-a-time cycle — `/next` → `/start` → `/task-done`, with `/phase-done` closing a
Phase. One Task per session, to keep context clean.
_Avoid_: "the workflow" (ambiguous — there are two loops).

**Orchestrator**:
The parallel runner — takes a Wave of parallel-safe Tasks, fans out one worktree + agent per
Task, integrates them behind a Review gate, and repeats until the Phase is done.
_Avoid_: "the loop" (ambiguous), batch (that names one Wave's contents, not the runner).

**Wave**:
A set of Tasks declared parallel-safe (disjoint Scopes) that the Orchestrator runs concurrently.
Tasks *within* a Wave are parallel; Waves run *sequentially*, because a later Wave may build on
an earlier one's merged output.
_Avoid_: batch, round, sprint.

### Task specification

**Scope** (scope statement):
The explicit in/out file boundary of a Task — what it may touch and what it must not. The
Orchestrator validates that the Scopes in a Wave do not overlap before fan-out.
_Avoid_: boundary, area.

**Brief**:
The self-contained, agent-readable spec handed to a worktree agent — locked Scope + Criterion +
conventions + relevant ADRs. Not the whole Plan.
_Avoid_: prompt, ticket.

**Current task**:
`docs/current-task.md` — the single-Task handoff the Sequential loop writes (in `/next`) and
reads (in `/start`).
_Avoid_: TODO, scratch file.

### Integration & review

**Integration branch**:
The throwaway branch a Wave's worktree branches merge into (smallest-first) to produce one
squash PR.
_Avoid_: develop, staging.

**Review gate**:
The human checkpoint after each Wave's integration PR, before the next Wave starts.
_Avoid_: approval, sign-off (those are outcomes of passing the gate).

### Records

**ADR**:
An Architecture Decision Record — one file in `docs/adr/`, capturing a hard-to-reverse,
non-obvious, traded-off decision. The project's only decision log.
_Avoid_: D-### entry, DECISIONS.md (the retired single-file convention this replaces).

**Codebase map**:
`docs/codebase-map.md` — the phase-organized index of where things live, updated as Tasks add
files.
_Avoid_: architecture doc, file index.

**Command**:
A user-invoked slash procedure in `.claude/commands/`; runs only when explicitly called (e.g.
`/next`). The Sequential loop and the Orchestrator are Commands.
_Avoid_: skill (a Skill triggers differently).

**Skill**:
A model-invocable capability in `.claude/skills/`; Claude may trigger it from its description, or
the user can call it. The vendored MP tools (tdd, grill-*, prototype, handoff, teach) are Skills.
_Avoid_: command (a Command runs only when explicitly invoked).

## Example dialogue

> **Dev**: Phase 3 has six unchecked Criteria. Can I run them as one Wave?
> **Lead**: Only if their Scopes are disjoint. Two of them both touch the auth middleware — same
> Scope, so they can't share a Wave.
> **Dev**: So a Wave of the other four, then a second Wave with the two auth ones?
> **Lead**: Right — Tasks inside a Wave are parallel, Waves are sequential. The Orchestrator fans
> out four worktrees, integrates them behind the Review gate, and only then starts the next Wave.
> **Dev**: And each worktree agent gets…?
> **Lead**: A Brief — the locked Scope, the Criterion, conventions, relevant ADRs. Not the whole
> Plan.
