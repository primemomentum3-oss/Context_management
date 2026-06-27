# Source Context Indexer Technical Spec

## Purpose

The indexer is a backend tool that scans a repository and creates the generated `.context` map.

It is not an agent. It does not solve tasks. It extracts facts.

## Required command

```text
npm run context:index
```

Recommended implementation:

```text
scripts/context-index.mjs
src/services/contextIndex.ts
src/domain/contextIndexTypes.ts
```

## Inputs

The indexer reads:

- repository file tree.
- package scripts.
- TypeScript/JavaScript imports and exports.
- test filenames and test folders.
- local README files.
- curated work areas.
- curated aliases.
- curated behaviors.
- ignore rules.

## Outputs

The indexer writes:

```text
.context/generated/files.json
.context/generated/imports.json
.context/generated/exports.json
.context/generated/symbols.json
.context/generated/tests.json
.context/generated/entrypoints.json
.context/generated/checks.json
.context/generated/graph.json
.context/generated/summary.json
```

## File discovery

The scanner should recursively list files and classify each as:

```text
source
test
doc
config
script
schema
generated
runtime
unknown
```

Default excluded paths:

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

## Import/export parser

For TypeScript and JavaScript, detect static imports:

```text
import ... from "..."
export ... from "..."
export * from "..."
require("...") where simple string literal
```

The first version does not need perfect AST coverage. It should be reliable for ordinary imports and honest about unknown dynamic imports.

## Test detection

Detect tests by:

```text
*.test.ts
*.spec.ts
tests/**
__tests__/**
```

Map tests to source by:

- direct imports.
- matching basename.
- same work area.
- curated behavior check list.

## Entrypoint detection

Detect likely entrypoints:

- CLI command files.
- MCP tool registry/tool files.
- API route files.
- UI screen/page files.
- worker/job files.
- event handler files.
- public package exports.

Each entrypoint should have:

```json
{
  "path": "src/cli/index.ts",
  "entrypoint_type": "cli",
  "work_area": "cli_runbooks_scripts",
  "calls": []
}
```

## Work-area assignment

Assignment order:

1. explicit curated path rule.
2. nearest local README.
3. folder naming match.
4. import cluster match.
5. unknown.

Unknown files must be reported.

## Graph creation

The graph should join:

- file nodes.
- import edges.
- test edges.
- work-area ownership.
- entrypoint edges.
- check commands.
- docs references.
- generated/runtime exclusions.

## Summary output

Create `.context/generated/summary.json`:

```json
{
  "generated_at": "2026-06-27T00:00:00Z",
  "repo": "agent-os",
  "file_count": 0,
  "source_count": 0,
  "test_count": 0,
  "unknown_work_area_count": 0,
  "entrypoint_count": 0,
  "warnings": []
}
```

## Check command

Add:

```text
npm run context:check
```

It should fail or warn when:

- generated files are stale.
- required generated files are missing.
- unknown work-area count exceeds threshold.
- entrypoints have no checks.
- runtime/build output is selected as source.

## Reliability expectations

Technical relationships:

```text
imports, exports, files, scripts, tests: 8-10/10 when language support is good
```

Human task relevance:

```text
first version: 6.5-7.5/10
good curated version: 8-8.5/10
excellent version with feedback loop: around 9/10
```

Perfect 10/10 should not be promised because hidden behavior, dynamic imports, vague names, and missing tests can exist.

## Implementation notes

Start simple. The first indexer should be useful, deterministic, and inspectable.

Do not block adoption by trying to infer perfect behavior semantics on day one.
