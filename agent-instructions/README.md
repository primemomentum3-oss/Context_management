# Agent Instructions

This folder contains task-ready instruction packets for implementation agents.

These are not essays. They are intended to be copied into an agent task as operating instructions.

## Files

| File | Purpose |
| --- | --- |
| `01-build-source-context-indexer.md` | Build the first `.context/generated` indexer. |
| `02-migrate-agent-os-docs.md` | Convert current Agent OS routing/docs into `.context` and redesign docs. |
| `03-run-agent-os-context-audit.md` | Audit Agent OS files/docs and produce unknown/stale/duplicate reports. |

## Agent execution rule

Every implementation agent should return:

```text
changed files
new generated files
checks run
unknown files found
missing context found
risk notes
```
