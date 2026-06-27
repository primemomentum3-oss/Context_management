# Context Management Standard

## Purpose

This standard defines how a codebase should expose source context to coding agents.

The goal is simple:

```text
human task -> routing agent -> exact context packet -> solving agent
```

The routing agent should not rely on random repository search. It should use a generated source context index plus a small curated meaning layer.

## Required structure

```text
.context/
  generated/
  curated/
  rules/
  packets/
```

## Generated layer

Generated files are produced by an indexer and should not be hand edited.

Required files:

```text
.context/generated/files.json
.context/generated/imports.json
.context/generated/exports.json
.context/generated/symbols.json
.context/generated/tests.json
.context/generated/entrypoints.json
.context/generated/graph.json
.context/generated/checks.json
```

These files answer factual questions:

- What files exist?
- Which files import other files?
- Which files export symbols?
- Which files are tests?
- Which files are entrypoints?
- Which checks exist?
- Which files are generated or runtime output?

## Curated layer

Curated files contain meaning the scanner cannot reliably infer.

Required files:

```text
.context/curated/work-areas.yaml
.context/curated/behaviors.yaml
.context/curated/aliases.yaml
```

This layer should stay small. It should not duplicate every generated relationship.

## Rules layer

Rules define how context is selected.

Required files:

```text
.context/rules/relevance-rules.yaml
.context/rules/risk-rules.yaml
.context/rules/maintenance-rules.yaml
```

## File card

Every source, test, config, script, and doc file should produce a file card.

Example:

```json
{
  "path": "src/example/exampleService.ts",
  "kind": "source",
  "language": "typescript",
  "work_area": "example",
  "roles": ["core_logic"],
  "exports": ["createExample"],
  "imports": ["src/example/exampleTypes.ts"],
  "imported_by": ["src/cli/exampleCommand.ts"],
  "entrypoint": false,
  "tests": ["tests/exampleService.test.ts"],
  "configs": [],
  "contracts": ["src/example/exampleTypes.ts"],
  "generated": false,
  "runtime": false
}
```

## Work area

A work area is a stable responsibility area.

Example:

```yaml
id: workflow
name: Workflow / Harness
plain_meaning: Backend traffic controller for task lifecycle.
aliases:
  - workflow
  - harness
  - lifecycle
owns:
  - workflow stage calculation
  - readiness checks
  - fix-pass routing
does_not_own:
  - final human decision
  - subjective review judgment
primary_paths:
  - src/domain/workflowStage.ts
  - src/services/workflow/
checks:
  - npm run workflow:smoke
  - tests/workflowStage.test.ts
```

## Behavior

A behavior is something the system does from start to finish.

Use behavior records when a task crosses multiple files or areas.

Example:

```yaml
id: profile_save_changes
work_area: user_profile
plain_meaning: User changes profile fields and persists them.
aliases:
  - profile does not save
  - old name still shown
  - save profile
entrypoints:
  - src/profile/ProfilePage.tsx
input_files:
  - src/profile/ProfileForm.tsx
core_logic:
  - src/profile/saveProfile.ts
contracts:
  - src/profile/profileTypes.ts
checks:
  - tests/profileSave.test.ts
```

## Relevance rules

A file is relevant only if the solving agent cannot safely understand, change, or verify the task without it.

For a bug fix, include:

```text
entrypoint
input/source of problem
wiring/call path
core logic
contract/data shape
configuration if behavior-changing
tests/checks
```

For a feature change, include:

```text
entrypoint
public contract
core logic
wiring/adapters
configuration if needed
tests/checks
```

For a refactor, include:

```text
target files
direct callers
direct dependencies
contracts
tests/checks
migration shims if public surfaces change
```

## Context packet

The routing agent must output a context packet.

Example:

```json
{
  "task": "Fix incorrect workflow readiness state.",
  "task_type": "bug_fix",
  "target_work_area": "workflow",
  "target_behavior": "workflow_ready_for_approval",
  "confidence": 0.86,
  "read_order": [
    {
      "path": "src/domain/workflowStage.ts",
      "role": "domain_rule",
      "reason": "Calculates lifecycle stage."
    }
  ],
  "checks": [
    {
      "command": "npm run workflow:smoke",
      "reason": "Exercises workflow lifecycle behavior."
    }
  ],
  "excluded": [
    {
      "path": "apps/desktop/src/App.tsx",
      "reason": "UI display only; backend lifecycle truth must be checked first."
    }
  ],
  "warnings": []
}
```

## Maintenance rule

Generated context updates automatically.

Curated context changes only when:

- a new work area is created.
- a work area is renamed, moved, merged, or retired.
- a cross-area behavior cannot be inferred reliably.
- routing repeatedly misses required files.
- source boundaries or safety boundaries change.

## Validation rule

CI should warn or fail when:

- a source file has no work area.
- an entrypoint has no mapped checks.
- generated context is stale.
- runtime or build-output folders are selected as source.
- a task packet has no checks.

## Quality metrics

Track:

```text
missing_file_rate
over_context_rate
wrong_area_rate
stale_context_rate
test_missing_rate
agent_rework_rate
```

A mature system should reduce missing files first, then reduce extra context.
