# agentic-workflow-template

A self-contained template repository for an agentic software-development workflow: a
disciplined **sequential task-loop**, a **parallel worktree orchestrator**, a curated set of
**vendored skills**, and the **documentation conventions** that tie them together. Start a new
project with **Use this template**.

> **Status: design in progress.** This repo is being built via a `grill-with-docs` design
> session. The decision records in [`docs/adr/`](docs/adr/) are settled; the commands and
> vendored skills land during implementation. Read the ADRs for the shape being built.

## What this is

Two complementary loops over one shared backbone:

- **Sequential loop** — one task per session, for careful, dependent, or uncertain work:
  `/next` → `/start` → `/task-done`, with `/phase-done` closing a phase.
- **Parallel orchestrator** — runs a *wave* of independent, pre-specced tasks concurrently in
  git worktrees, integrates them behind a per-wave review gate, and repeats until the phase is
  done.

Plus a curated set of skills (test-first, design-grilling, prototyping, handoff, teaching)
borrowed from [Matt Pocock's skills](https://github.com/mattpocock/skills), mapped onto the
loop's phases.

The shared vocabulary for all of the above lives in [`CONTEXT.md`](CONTEXT.md).

## Structure (target)

```
.
├── CONTEXT.md              # workflow glossary (the shared vocabulary)
├── docs/
│   └── adr/                # one file per architectural decision
├── .claude/
│   ├── commands/           # the loop: next, start, task-done, phase-done, review, + orchestrator
│   └── skills/             # vendored skills (tdd, grill-*, prototype, handoff, teach, parallel-batch)
└── plans/                  # phased acceptance-criteria plans (incl. parallel-safe waves)
```
(Directories appear as they are built.)

## Using it for a new project

1. **Use this template** on GitHub (or copy the repo).
2. Fill in the project-specific hooks: the test/verify command and the review rules.
3. Write a plan in `plans/`, then drive it with the sequential loop or the orchestrator.

## Credits & license

This repo vendors skills from [github.com/mattpocock/skills](https://github.com/mattpocock/skills)
(MIT). Vendored skill directories retain their upstream MIT license and attribution. This
repository is itself MIT-licensed — see [`LICENSE`](LICENSE).
