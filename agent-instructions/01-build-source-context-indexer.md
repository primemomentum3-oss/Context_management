# Build Source Context Indexer

## Objective

Implement the first `.context/generated` indexer in Agent OS.

## Add files

```text
scripts/context-index.mjs
src/domain/contextIndexTypes.ts
src/services/contextIndex.ts
tests/contextIndex.test.ts
.context/generated/README.md
```

## Add package scripts

```text
context:index
context:check
```

## Generate

```text
.context/generated/files.json
.context/generated/imports.json
.context/generated/tests.json
.context/generated/entrypoints.json
.context/generated/checks.json
.context/generated/graph.json
.context/generated/summary.json
```

## Required behavior

1. List repository files.
2. Classify file kind.
3. Skip build and runtime output.
4. Parse simple TypeScript imports and exports.
5. Detect tests by path and name.
6. Read `package.json` scripts.
7. Detect local README files.
8. Assign files to work areas from curated metadata.
9. Report unknown files.

## Paths to skip as source

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

## Tests

Add tests for:

- file classification.
- import extraction.
- test detection.
- skipped paths.
- work-area assignment.
- stale index detection.

## Completion report

Return:

```text
changed files
commands run
sample generated summary
unknown files count
limitations
next recommended improvement
```

## Non-goals

Do not add embeddings in the first pass.

Do not rewrite docs in this task.

Do not restructure source folders.
