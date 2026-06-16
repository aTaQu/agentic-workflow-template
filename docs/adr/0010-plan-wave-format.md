# Plans express parallelism as opt-in wave subsections

A plan phase is a flat `- [ ]` criterion list by default (sequential). To parallelize, criteria
are grouped under `### Wave N (parallel)` subsections, each task carrying a `Scope: in <globs>`
line (an `out` may be given; it defaults to "everything else"). The sequential loop's `/next`
ignores wave headers and walks unchecked criteria in order; the Orchestrator runs only phases that
declare `(parallel)` waves and validates the declared scopes don't overlap before fan-out.
Considered inline `(wave:X)` tags (compact, but parallelism is invisible at a glance and tags are
typo-fragile) and a separate YAML manifest (machine-clean, but splits the plan across two files
that drift). Chose wave subsections to keep one source of truth and make independence — the
riskiest property — a visible structural unit.
