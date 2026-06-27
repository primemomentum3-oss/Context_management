# Agent OS Implementation Plan For Source Context Management

## Goal

Add a first-class context layer to Agent OS so every meaningful task can produce a source context packet before an agent starts work.

Target flow:

```text
user request
  -> intake/classification
  -> context routing
  -> context packet
  -> scoped agent work
  -> verification/review/evidence
```

## Principle

Start additive. Do not begin with a broad cleanup.

```text
add indexer
add generated context
add curated work areas
add routing packet
then clean weak areas revealed by the index
```

## Phase 1: Add `.context`

Create:

```text
.context/generated/
.context/curated/
.context/rules/
.context/packets/
```

Seed curated work areas from the current Agent OS work-area router:

- ui_operator_workbench
- projects_tasks_worktrees
- intake_classification
- workflow_harness
- sessions_runs_launchers
- mcp_tool_gateway
- hooks_automatic_capture
- evidence_artifacts_trace
- validation_review_approval
- memory_learning_evals
- database
- cli_runbooks_scripts

Acceptance criteria:

- every work area has id, aliases, primary paths, checks, owns, does_not_own, and boundary rules.
- the Markdown router remains as a human shortcut but is no longer the only routing source.

## Phase 2: Build source context indexer

Add:

```text
scripts/context-index.mjs
src/services/contextIndex.ts
src/domain/contextIndexTypes.ts
tests/contextIndex.test.ts
```

Generated outputs:

```text
.context/generated/files.json
.context/generated/imports.json
.context/generated/tests.json
.context/generated/entrypoints.json
.context/generated/checks.json
.context/generated/graph.json
```

Minimum behavior:

- list repository files.
- ignore runtime/build-output folders.
- classify files by kind.
- parse TypeScript imports and exports.
- detect tests by path and name.
- read package scripts.
- detect local README files.
- assign files to work areas from curated metadata.

Acceptance criteria:

- `npm run context:index` writes generated context.
- `npm run context:check` confirms generated context is current.
- each source file is assigned to a work area or reported as unknown.

## Phase 3: Add routing service

Add:

```text
src/services/contextRouting.ts
src/domain/contextRoutingTypes.ts
tests/contextRouting.test.ts
```

Input:

```json
{
  "task": "Improve workflow readiness routing.",
  "task_type": "bug_fix",
  "constraints": {
    "include_tests": true,
    "max_files": 20
  }
}
```

Output:

```json
{
  "target_work_area": "workflow_harness",
  "target_behavior": null,
  "confidence": 0.78,
  "read_order": [],
  "checks": [],
  "excluded": [],
  "warnings": []
}
```

Acceptance criteria:

- broad Agent OS phrases map to the correct work area.
- work areas return primary files, tests, checks, and local README.
- runtime/build-output folders are never selected as source.

## Phase 4: Integrate context packets

Update task/run/handoff creation so meaningful work receives a packet containing:

- task id.
- user request.
- task type.
- target work area.
- target behavior if known.
- files to read.
- reasons.
- checks.
- warnings.
- confidence.
- generated context version.

Likely integration points:

```text
src/services/runtime.ts
src/services/tasks/createTaskRecord.ts
src/services/launcher.ts
src/services/automaticExecutor.ts
```

Acceptance criteria:

- created tasks can store or reference a source context packet.
- agent launch input includes the packet.
- evidence/reporting can show which context was provided.

## Phase 5: Redesign documentation layer

Classify docs into:

```text
source_of_truth
local_area_navigation
runbook
spec
historical_archive
stale_candidate
```

Keep active docs short and structured.

Move machine-readable routing into `.context`.

Local README files should own detailed area navigation.

Acceptance criteria:

- default agent read path is shorter.
- broad Markdown routing is backed by machine-readable context.
- stale docs are outside the default read path.

## Phase 6: Add quality gates

Add scripts:

```text
npm run context:index
npm run context:check
npm run context:route:smoke
npm run context:docs:audit
```

Checks should warn or fail when:

- generated context is stale.
- source files are unmapped.
- runtime/build-output folders are selected as source.
- a context packet has no checks.
- default-read docs are stale.

## Phase 7: Add feedback loop

When a solving agent needed a file missing from the packet, record:

```json
{
  "task_id": "...",
  "missing_file": "src/example/file.ts",
  "why_needed": "direct dependency of selected file",
  "routing_gap_type": "missing_dependency"
}
```

Use this to improve the indexer, curated metadata, and relevance rules.

## Definition of done

- source files are mapped to work areas or reported as unknown.
- major work areas have curated metadata.
- meaningful tasks produce context packets.
- packets include files, reasons, checks, warnings, and confidence.
- runtime/build-output folders are excluded.
- docs are classified and default read path is clean.
- routing quality is measured over time.
