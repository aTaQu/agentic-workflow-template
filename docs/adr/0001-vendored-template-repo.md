# Vendor skills into a self-contained template repo

The agentic workflow ships as a self-contained Git **template repo** that carries its own copies
of the chosen skills, the task-loop commands, and the doc templates; new projects start via
GitHub "Use this template". Considered relying on globally-installed skills (DRY, but a new
project's behavior then depends on invisible per-machine state and isn't portable) and a
plugin + marketplace (best for wide distribution, but too much machinery for personal reuse).
Chose vendoring for reproducibility on any machine and zero dependency on global setup; the cost
is that each project's skill copies drift from upstream and must be re-synced periodically.
