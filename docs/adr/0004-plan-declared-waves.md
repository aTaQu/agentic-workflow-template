# Parallel tasks are selected as plan-declared, validated waves

Tasks are grouped into explicit parallel-safe **Waves** with declared in/out Scopes at planning
time; the Orchestrator pulls a Wave and validates that the Scopes don't overlap, surfacing any
conflict for confirmation before fan-out. Considered handing the Orchestrator a human-picked
batch each run (simplest, but no Plan integration and independence rests on memory) and
auto-selecting a maximal independent set from the Plan (most automation, but independence
detection from specs is fragile — one wrong call silently corrupts a whole batch of worktrees).
Chose plan-declared + validate as the safe middle: the parallelism decision lives where the
grilling already happens.
