# openclaw-task-continuation

A multi-agent long-task continuation workflow for OpenClaw.

## What this is

This repo captures a practical workflow for preventing long-running agent tasks from stalling after one step.

It uses a simple 4-agent split:

- `main` — orchestration, routing, summaries
- `monkey` — content execution
- `panda` — research / information gathering
- `assi` — supervision, stale detection, continuation intent

## What is already working

- Shared task board for long-running tasks
- Documented task lifecycle and continuation rules
- `monkey` execution path validated under `MiniMax-M2.5`
- `assi` supervision path validated
- Supervised continuation design documented
- A working auto-trigger v1 approach using a continuation worker cron job

## What is not fully finished yet

The elegant full automatic chain:

`assi -> main -> monkey`

is documented, but the currently stable automatic version is the simpler v1 worker-based approach.

## Repo layout

- `MULTI_AGENT_WORKFLOW.md` — top-level workflow entrypoint
- `context/` — orchestration and routing docs
- `tasks/` — task board format, workflow, continuation spec, examples
- `workspace-assi-context/` — supervision / continuation docs for assi
- `workspace-monkey-context/` — execution role notes for monkey
- `workspace-panda-context/` — research role notes for panda

## Intended use

These files are designed to be adapted into an OpenClaw workspace rather than used as a standalone app.

## Notes

Paths in the documents currently reflect the original local OpenClaw workspace layout and may need adjustment for reuse.
