# Routing Agent Contract

## Purpose

The routing agent chooses which files a solving agent should read for a task.

The routing agent should not solve the task. It should return a context packet.

## Inputs

The routing agent receives:

```json
{
  "task": "The workflow says ready for approval even when verification failed.",
  "task_type": "bug_fix",
  "repo": "agent-os",
  "constraints": {
    "max_files": 20,
    "include_tests": true,
    "include_docs": true
  }
}
```

It may read:

```text
.context/generated/graph.json
.context/generated/files.json
.context/generated/tests.json
.context/generated/entrypoints.json
.context/generated/checks.json
.context/curated/work-areas.yaml
.context/curated/behaviors.yaml
.context/curated/aliases.yaml
.context/rules/relevance-rules.yaml
.context/rules/risk-rules.yaml
```

It should not read full source files unless explicitly allowed by the execution mode.

## Required output

```json
{
  "task": "The workflow says ready for approval even when verification failed.",
  "task_type": "bug_fix",
  "target_work_area": "workflow_harness",
  "target_behavior": "workflow_ready_for_approval",
  "confidence": 0.84,
  "read_order": [
    {
      "path": "src/domain/workflowStage.ts",
      "role": "domain_rule",
      "reason": "Calculates workflow stage and readiness state."
    },
    {
      "path": "src/services/workflow/closerPackages.ts",
      "role": "core_logic",
      "reason": "Creates and checks closer package readiness."
    }
  ],
  "checks": [
    {
      "command": "npm run workflow:smoke",
      "reason": "Covers workflow lifecycle behavior."
    },
    {
      "command": "tests/workflowStage.test.ts",
      "reason": "Covers workflow stage rules."
    }
  ],
  "excluded": [
    {
      "path": "apps/desktop/",
      "reason": "UI is not the source of lifecycle truth."
    }
  ],
  "warnings": []
}
```

## Selection algorithm

1. Parse task language.
2. Extract anchors: area words, behavior words, error words, file names, commands, routes, tests, and domain terms.
3. Match anchors to work-area aliases.
4. Match anchors to behavior aliases.
5. Determine task type.
6. Apply relevance rules for that task type.
7. Select files by role: entrypoint, core logic, contracts, config, tests, docs if needed.
8. Add direct dependencies and direct callers when required.
9. Exclude generated/runtime/build-output folders.
10. Return packet with reasons and confidence.

## Confidence scoring

Confidence should consider:

- exact behavior match.
- exact work-area match.
- presence of tests.
- presence of entrypoint.
- number of unknown files in the area.
- whether the task is vague.
- whether multiple areas match equally.

Suggested bands:

```text
0.90-1.00: exact behavior and file roles known
0.75-0.89: clear work area and likely files
0.55-0.74: broad area known, file precision uncertain
0.00-0.54: insufficient routing confidence
```

## Missing context behavior

If confidence is low, do not guess silently.

Return:

```json
{
  "confidence": 0.52,
  "warnings": [
    "Multiple work areas matched: workflow_harness and validation_review_approval.",
    "No exact behavior record found."
  ]
}
```

## Exclusion rule

The routing agent must explicitly exclude files that look related but should not be read first.

Example:

```json
{
  "path": "apps/desktop/src/App.tsx",
  "reason": "Visible status display only; backend workflow state is source of truth."
}
```

## Feedback rule

If the solving agent later needs an omitted file, it should report the missing file and why it was needed.

This feedback should update either:

- generated indexer logic.
- curated behavior metadata.
- relevance rules.
- local README clarity.

## Non-goals

The routing agent must not:

- edit code.
- decide product behavior.
- invent file contents.
- claim certainty when context is missing.
- select generated/runtime folders as source.
