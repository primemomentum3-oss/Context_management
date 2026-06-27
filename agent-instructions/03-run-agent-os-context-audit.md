# Run Agent OS Context Audit

## Objective

Audit Agent OS after `.context` exists and report where context quality is weak.

## Audit outputs

Create reports:

```text
.context/generated/summary.json
.context/generated/unknown-files.json
.context/generated/docs-audit.json
.context/generated/entrypoint-checks.json
.context/generated/context-quality-report.json
```

## Check 1: unknown files

List source files that are not assigned to a work area.

Each item should include:

```json
{
  "path": "src/example/file.ts",
  "reason": "No curated path rule matched."
}
```

## Check 2: stale docs

List docs that are likely stale, duplicated, or historical.

Each item should include:

```json
{
  "path": "docs/example.md",
  "classification": "stale_candidate",
  "reason": "Not in current read path and overlaps with structured context."
}
```

## Check 3: entrypoints without checks

List entrypoints without tests or smoke commands.

## Check 4: broad files

List files that may be too broad for precise routing.

Suggested thresholds:

```text
400+ lines: split consideration when touched
600+ lines: split plan recommended before adding behavior
```

## Check 5: packet smoke

Run sample routing tasks for each work area and verify packets include:

- target work area.
- files.
- reasons.
- checks.
- exclusions.
- confidence.

## Completion report

Return:

```text
unknown source files
entrypoints without checks
stale docs candidates
large files
routing smoke results
top cleanup priorities
```
