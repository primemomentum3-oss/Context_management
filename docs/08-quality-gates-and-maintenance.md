# Quality Gates And Maintenance

## Purpose

A context management system only works if the index is current, trusted, and measured.

The goal is not to create another documentation folder. The goal is to create a maintained operational layer.

## Required commands

```text
npm run context:index
npm run context:check
npm run context:route:smoke
npm run context:docs:audit
```

## Gate 1: Generated context is current

`context:check` should compare current repo state against generated `.context/generated/*` outputs.

Fail when:

- generated files are missing.
- generated files are stale.
- source files changed but index was not regenerated.

## Gate 2: Source files are mapped

Warn or fail when source files are not assigned to a work area.

Example report:

```json
{
  "unknown_work_area_files": [
    "src/newFeature/newService.ts"
  ]
}
```

## Gate 3: Generated/runtime folders are excluded

Fail if the routing system selects these as source:

```text
dist/
node_modules/
coverage/
.agent-os/
apps/*/dist/
apps/*/node_modules/
*.log
.DS_Store
```

## Gate 4: Entrypoints have checks

Warn when an entrypoint has no mapped check.

Example:

```json
{
  "entrypoint": "src/mcp/server.ts",
  "mapped_checks": []
}
```

## Gate 5: Context packets have checks

A context packet for meaningful implementation work should include checks.

Warn when:

- no tests are selected.
- only docs are selected.
- no verification command exists.

## Gate 6: Documentation status is classified

The docs audit should classify documentation into:

```text
source_of_truth
local_area_navigation
runbook
contract_spec
historical_archive
status_report
stale_candidate
```

Warn when:

- stale candidates appear in default read path.
- a work area has no local README.
- a docs file duplicates machine-readable context.

## Maintenance model

Generated context is automatic.

Curated context changes only when:

- work areas change.
- behavior meaning changes.
- alias/routing words change.
- repeated routing failures occur.
- safety or boundary rules change.

## Feedback metrics

Track:

```text
missing_file_rate
over_context_rate
wrong_area_rate
stale_context_rate
test_missing_rate
agent_rework_rate
```

## Target quality levels

Initial useful level:

```text
source files mapped: >= 80%
missing_file_rate: measured, not yet optimized
runtime exclusions: 100%
context packets include checks: >= 80%
```

Good level:

```text
source files mapped: >= 95%
missing_file_rate: < 10%
over_context_rate: < 30%
wrong_area_rate: < 5%
context packets include checks: >= 95%
```

Excellent level:

```text
source files mapped: >= 98%
missing_file_rate: < 5%
over_context_rate: < 20%
wrong_area_rate: < 2%
context packets include checks: >= 98%
```

## Rule of thumb

Optimize in this order:

1. Avoid missing critical files.
2. Avoid wrong work area.
3. Include tests/checks.
4. Reduce extra files.
5. Improve curated behavior mapping.

Do not optimize for tiny context packets before the system reliably includes the files needed to solve tasks.
