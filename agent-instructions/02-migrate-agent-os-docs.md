# Migrate Agent OS Docs To Structured Context

## Objective

Move Agent OS routing information from broad Markdown-only docs into `.context` while keeping human docs short and current.

## Inputs

Read:

```text
README.md
docs/HANDOFF.md
docs/current-status.md
docs/prds/README.md
docs/prds/00-agent-os-work-area-index-and-navigation-prd.md
docs/agent/work-area-router.md
local README files in source areas
```

## Create or update

```text
.context/curated/work-areas.yaml
.context/curated/aliases.yaml
.context/curated/behaviors.yaml
.context/rules/relevance-rules.yaml
.context/rules/maintenance-rules.yaml
docs/context-transition-status.md
```

## Work-area conversion

Convert each work area into structured metadata:

```yaml
id: workflow_harness
name: Workflow / Harness
aliases:
  - workflow
  - harness
  - lifecycle
primary_paths:
  - src/domain/workflowStage.ts
  - src/services/workflow/
checks:
  - npm run workflow:smoke
owns: []
does_not_own: []
boundary_rules: []
```

## Documentation cleanup rules

Do not remove first. Classify first.

Classify docs as:

```text
source_of_truth
local_area_navigation
runbook
contract_spec
historical_archive
status_report
stale_candidate
```

## Expected result

The default context path should become shorter and more structured:

```text
README.md
.context/curated/work-areas.yaml
.context/rules/relevance-rules.yaml
relevant local README
relevant runbook only when checks are needed
```

## Completion report

Return:

```text
converted work areas
new curated files
docs classified
stale candidates
open questions
checks run
```
