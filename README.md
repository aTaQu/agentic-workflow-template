# agentic-workflow-template

A self-contained template repository for an agentic software-development workflow: a disciplined
**sequential task-loop**, a **parallel worktree orchestrator**, a curated set of **vendored skills**,
and the **documentation conventions** that tie them together. Start a new project with
**Use this template**.

The design rationale is recorded as ADRs in [`docs/adr/`](docs/adr/) (0001–0013).

## The two loops

One backbone, two modes — pick by the work:

- **Sequential loop — the *control* path.** One task per session, maximally interactive:
  `/next` → `/start` → `/task-done`, with `/phase-done` closing a phase. For dependent, uncertain, or
  careful work.
- **Parallel orchestrator — the *speed* path.** `/orchestrate` runs a *wave* of independent,
  pre-specced tasks concurrently in git worktrees, integrates them behind a per-wave review gate, and
  repeats. For batches of independent work.

Both are soft-wired to the vendored skills: `/start` nudges `/tdd`; design starts with
`/grill-with-docs`; `/handoff` when a session fills. Shared vocabulary lives in
[`CONTEXT.md`](CONTEXT.md).

## Structure

```
AGENTS.md                 # canonical per-project config (Verify hook, Review rules, workflow, rules)
CLAUDE.md                 # imports AGENTS.md so Claude Code loads it natively
CONTEXT.md                # workflow glossary (extend with your domain terms)
docs/
  adr/                    # one file per decision (0001–0013 = this template's own design)
  codebase-map.md         # phase-organized index, kept current by the loop
plans/                    # phased acceptance-criteria plans; 0002 is a worked example
.claude/
  commands/               # next, next-search, start, task-done, phase-done, review, diff-review,
                          #   next-wave, orchestrate
  skills/                 # 9 vendored skills (+ ATTRIBUTION.md, LICENSE-mattpocock)
```

## Using it for a new project

1. **Use this template** on GitHub (or copy the repo).
2. Fill in `AGENTS.md` — the `TODO`s: project description, the **Verify** commands
   (typecheck/lint/test), and your **Review rules**.
3. Reset the docs: clear `docs/adr/` (keep its README), trim `CONTEXT.md` to your domain, replace
   `plans/` with your own (delete `0001`/`0002`).
4. Write a plan, then drive it: `/next` for sequential work, `/orchestrate` for parallel waves.

## Commands

| Command | Use |
|---|---|
| `/next` | Pick the next criterion → `docs/current-task.md` (throwaway session) |
| `/start` | Implement the current task (fresh session; nudges `/tdd`) |
| `/task-done` | Verify, review, mark, commit, chain to `/next` |
| `/phase-done` | Close a phase: full review, update map, handoff |
| `/review [path]` | Standalone diff review |
| `/orchestrate` | Run the next parallel wave from the plan |

## Credits & license

Vendors skills from [github.com/mattpocock/skills](https://github.com/mattpocock/skills) (MIT) — see
[`.claude/skills/ATTRIBUTION.md`](.claude/skills/ATTRIBUTION.md). This repository is MIT-licensed —
see [`LICENSE`](LICENSE).
