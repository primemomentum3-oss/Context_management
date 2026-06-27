# Context Packet Contract

## Purpose

A context packet is the handoff from the routing agent to the solving agent.

It answers:

```text
What should the solving agent read first, why, and how should the result be verified?
```

## Required fields

```json
{
  "schema_version": "1.0",
  "generated_at": "2026-06-27T00:00:00Z",
  "repo": "agent-os",
  "task": {
    "id": "optional-task-id",
    "title": "short task title",
    "user_request": "original human request",
    "task_type": "bug_fix"
  },
  "routing": {
    "target_work_area": "workflow_harness",
    "target_behavior": "workflow_ready_for_approval",
    "confidence": 0.84,
    "index_version": "context-index-hash"
  },
  "read_order": [],
  "checks": [],
  "excluded": [],
  "warnings": []
}
```

## Read order item

```json
{
  "path": "src/domain/workflowStage.ts",
  "kind": "source",
  "role": "domain_rule",
  "reason": "Calculates workflow stage.",
  "required": true
}
```

Allowed roles:

```text
entrypoint
user_input
api_entrypoint
cli_entrypoint
mcp_entrypoint
worker_entrypoint
event_handler
core_logic
domain_rule
service_logic
adapter
contract
schema
config
database_migration
test
runbook
local_readme
```

## Check item

```json
{
  "command": "npm run workflow:smoke",
  "type": "smoke",
  "reason": "Covers workflow lifecycle behavior.",
  "requires_disposable_data": false
}
```

## Excluded item

```json
{
  "path": "dist/",
  "reason": "Generated build output. Do not edit as source."
}
```

## Warning item

```json
{
  "level": "medium",
  "message": "No exact behavior record matched. Routing used work-area fallback."
}
```

## Solving agent requirements

The solving agent must:

1. Read files in the provided order.
2. Avoid excluded paths unless later evidence proves they are needed.
3. Preserve the target work area unless the task clearly crosses areas.
4. Run or recommend listed checks.
5. Report missing context when additional files were required.

## Missing context report

```json
{
  "task_id": "...",
  "missing_file": "src/example/file.ts",
  "why_needed": "Called by selected core logic file.",
  "routing_gap_type": "missing_direct_dependency"
}
```

Allowed gap types:

```text
missing_direct_dependency
missing_caller
missing_test
missing_config
missing_contract
wrong_work_area
ambiguous_behavior
stale_index
missing_curated_metadata
```

## Packet quality rules

A packet is poor if:

- it has no tests or checks.
- it contains only docs.
- it includes generated/runtime paths as source.
- it has no inclusion reasons.
- it includes more than 25 files without grouping.
- it lacks confidence or warnings.

A packet is good if:

- it has a clear target work area.
- it includes source files and tests.
- it explains why each file matters.
- it excludes tempting but wrong areas.
- it states confidence honestly.
