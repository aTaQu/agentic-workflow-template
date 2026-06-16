# The sequential loop stays human-paced; the orchestrator is the speed path

The sequential loop keeps its human-paced, one-task-per-session model: a throwaway `/next` selects
and writes the task, the developer starts a fresh `/start` session and implements it
interactively, `/task-done` runs automatically, and it auto-chains back to `/next`. The developer
stays in each `/start` session with full control to steer mid-task. Considered an auto-looping
subagent driver (next → subagent implements → next, until the phase is done — less session
juggling, unified with the orchestrator) and a hybrid auto-loop with a per-task gate. Chose
human-paced to make the division of labor explicit: the sequential loop is the *control* path
(maximally interactive), the parallel orchestrator is the *speed* path.
