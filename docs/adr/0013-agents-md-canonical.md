# AGENTS.md is canonical config; CLAUDE.md imports it

Per-project config — project instructions plus the verify-command and review-rules hooks the
commands read — lives in `AGENTS.md` (the emerging cross-tool standard), and `CLAUDE.md` is a thin
pointer that imports it (`@AGENTS.md`) so Claude Code loads it natively every session. Considered
CLAUDE.md as canonical (Claude-native but not portable to other agent tools) and AGENTS.md alone
(portable but not loaded natively by Claude Code). Chose AGENTS.md-canonical + CLAUDE.md-pointer to
get cross-tool portability and native Claude Code loading, at the cost of one extra one-line file.
