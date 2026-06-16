# Orchestration is continuous with a per-wave review gate

The Orchestrator runs each Wave hands-off (fan out → implement → verify → integrate to one squash
PR), then **stops for a human Review gate** before pulling the next Wave, repeating until the
Phase is done. Tasks within a Wave are parallel; Waves are sequential because a later Wave may
build on an earlier Wave's merged output. Considered one-shot (re-invoke per Wave — most control,
most manual) and fully continuous (no gate — fastest, but unreviewed integrations stack up and a
bad Wave poisons later ones). Chose continuous-with-gate to keep speed within a Wave and a safety
checkpoint between Waves.
