# A parallel orchestrator built on a genericized parallel-batch

Alongside the sequential loop, the boilerplate ships a parallel **Orchestrator** that implements
multiple pre-specced Tasks concurrently in git worktrees, to go faster on independent work. It
reuses the existing `parallel-batch` pipeline (grill → scout → worktree fan-out → TDD agents →
detached-checkout verify → integration squash PR → cleanup), genericized off the PHP/Symfony
stack into a per-project test/verify hook. The Orchestrator's only net-new part over
`parallel-batch` is a front end that feeds it Tasks **from the Plan** instead of fresh feature
ideas — so the grilling discipline moves upstream into planning.
