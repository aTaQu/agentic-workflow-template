# <Project Name>

> **This is the per-project config for the agentic workflow.** `CLAUDE.md` imports it, so Claude
> Code loads it automatically. Replace the `TODO` sections when you start a new project; the
> **Workflow** and **Rules** sections are the template's engine — leave them unless you mean to
> change how the loop behaves.

## Project

TODO — one paragraph: what this project is and why it exists.
Stack: TODO.

## Verify

The commands that prove a change is sound. `/task-done` runs these, and the orchestrator runs them
as each parallel task's cheap per-task gate. Use whatever your stack provides; delete lines that
don't apply.

- Typecheck: `TODO` (e.g. `npm run typecheck`, `mypy .`, `vendor/bin/phpstan analyse`)
- Lint: `TODO` (e.g. `npm run lint`)
- Tests: `TODO` (e.g. `npm test`, `pytest`, `php bin/phpunit`)

## Review rules

Project-specific dimensions `diff-review` checks, **in addition to** the generic ones it always
applies (ADR drift, scope adherence, test coverage, security, codebase-map drift). Replace these
examples with yours, or delete the section if the generics suffice.

- TODO (e.g. "Every user-facing string uses i18n; new keys exist in all locale files")
- TODO (e.g. "No raw SQL with user input — parameterized queries only")
- TODO (e.g. "Availability queries filter `is_active = true`")

## Workflow

A design-grill → document (ADR) → test-first → review loop, in two modes. Vocabulary is defined in
[`CONTEXT.md`](CONTEXT.md); decisions are recorded one-file-per-ADR in [`docs/adr/`](docs/adr/).

### Sequential loop — the *control* path (one task per session)

- `/next` — pick the next unchecked criterion from `plans/`, write `docs/current-task.md`, stop. Run in a throwaway session.
- `/start` — implement what's in `docs/current-task.md`. Run in a fresh session. **For non-trivial behavior, run `/tdd`** (red → green → refactor).
- `/task-done` — run **Verify**, delegate a `diff-review`, mark the criterion, update the codebase map, commit, add an ADR if one was flagged, then chain to `/next`.
- `/phase-done` — when every criterion in the phase is checked: full review, update the codebase map, commit docs, output a handoff.
- `/review [path]` — standalone `diff-review` without committing.

### Parallel orchestrator — the *speed* path (a wave of independent tasks)

- `/orchestrate` — pull the next `### Wave N (parallel)` from the active plan, validate the declared
  Scopes don't overlap, fan out one git worktree + agent per task (each runs `/tdd` + the per-task
  **Verify** gate), integrate the wave into one squash PR, then **stop at the review gate**. On your
  approval it pulls the next wave, until the phase is done.

Plans declare parallel-safe groups as `### Wave N (parallel)` subsections with a per-task
`Scope: in <globs>` line. A phase with no wave subsections is fully sequential. See `plans/`.

## Rules

- **Design first.** Resolve design branches with `/grill-with-docs` (or `/grill-me` if there's no
  doc convention yet) *before* building. Capture domain terms in `CONTEXT.md` as they're settled.
- **Record decisions as ADRs.** A non-obvious, hard-to-reverse, genuinely-traded-off choice → one
  new file in `docs/adr/` (`NNNN-slug.md`). Don't reopen settled ADRs silently.
- **One task = one acceptance criterion** by default. Split only by the objective tests in `/next`.
  Never batch multiple tasks into one commit; never commit without `/task-done`.
- **Stay in scope.** Touch only what the task declares. Surface unrelated issues; don't fix them inline.
- **Context budget.** When a session fills up, `/handoff` to compact it for a fresh session.
- **Security.** No secrets in code or fixtures. No raw user input in queries. CSRF/auth on state-changing operations.

## Working principles

Behavioral discipline for every task — adapted from Andrej Karpathy's coding guidelines
([multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills)). Bias
toward caution over speed; for trivial tasks, use judgment.

1. **Think before coding.** State assumptions explicitly; if uncertain, ask. When multiple
   interpretations exist, surface them — don't silently pick one. If a simpler approach exists, say
   so. When something is unclear, stop and name it.
2. **Simplicity first.** The minimum code that solves the problem, nothing speculative — no
   unrequested features, no abstractions for single-use code, no error handling for impossible
   cases. If 200 lines could be 50, rewrite. Ask: "would a senior engineer call this overcomplicated?"
3. **Surgical changes.** Touch only what the task needs. Don't reformat or "improve" adjacent code,
   don't refactor what isn't broken, match the existing style. Remove orphans *your* change created;
   leave pre-existing dead code (mention it, don't delete it). Every changed line should trace to the
   request.
4. **Goal-driven execution.** Turn the task into a verifiable goal and loop until it's met — "add
   validation" becomes "write tests for invalid inputs, then make them pass." For multi-step work,
   state a brief plan with a verify check per step. (`/tdd` and the **Verify** hook operationalize this.)
5. **Verify before you hand it over.** Resolve routes, commands, CLI flags, config keys, and env vars
   from source — route definitions, the tool's own `--help` / list command, config files — never
   invent them from naming patterns or "what it probably is." If the user will run or click it, check
   at least one first.
