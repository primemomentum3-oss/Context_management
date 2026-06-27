# Agent OS Documentation Redesign

## Purpose

This document defines how Agent OS documentation should be redesigned so it becomes suitable for AI agents.

The goal is not more human documentation.

The goal is:

```text
less narrative
less duplication
less stale planning history
more structured routing
more local ownership
more machine-readable context
```

## Current problem pattern

Agent OS currently uses documentation as the main context layer. That creates confusion because different document types are mixed:

- current instructions.
- PRDs.
- specs.
- runbooks.
- historical planning.
- certification reports.
- test logs.
- area READMEs.
- transition routers.

For agents, this creates risk:

- too many files to read before doing work.
- stale docs may appear authoritative.
- routing information is duplicated.
- broad docs become too large.
- exact file selection still requires guessing.

## Target documentation model

Agent OS should use three layers:

```text
1. Machine-readable context: .context/
2. Short active docs: docs/
3. Local area navigation: README files beside source areas
```

## New default read path

Recommended future read path for agents:

```text
README.md
.context/curated/work-areas.yaml
.context/rules/relevance-rules.yaml
.context/generated/graph.json
relevant local README
relevant runbook only when executing checks
```

Human-facing summary docs may remain, but they should not be required for precise file routing.

## Documentation categories

Every documentation file should be classified as one of these:

```text
source_of_truth
local_area_navigation
runbook
contract_spec
historical_archive
status_report
stale_candidate
```

## What stays in `docs/`

Keep only documentation that is current, short, and useful:

- current status.
- high-level handoff.
- active runbooks.
- active specs.
- certification reports.
- documentation audit output.

## What moves to `.context`

Move machine-usable information to `.context`:

- work-area maps.
- aliases.
- source path ownership.
- behavior maps.
- routing rules.
- generated file inventories.
- checks mapped to work areas.

## What local READMEs should contain

Every stable area should have a local README with:

```text
owns
does not own
main entrypoints
important invariants
events emitted/consumed if relevant
tests/checks
common mistakes
```

Local READMEs should stay close to the code they describe.

## What should be removed from default agent context

Remove from default read path:

- old planning notes.
- superseded implementation plans.
- raw logs.
- duplicate work-area tables.
- stale PRD gap language after implementation is complete.
- historical reviews that are not active instructions.

These can be archived or retained as historical context, but they should not guide normal agent work.

## Redesign steps

### Step 1: Classify docs

Build `docs/context-doc-audit.json` or `.context/generated/docs.json` containing:

```json
{
  "path": "docs/prds/00-agent-os-work-area-index-and-navigation-prd.md",
  "classification": "source_of_truth",
  "current": true,
  "default_read_path": true,
  "machine_replacement": ".context/curated/work-areas.yaml"
}
```

### Step 2: Convert routing tables

Convert Markdown work-area tables into `.context/curated/work-areas.yaml`.

### Step 3: Shorten broad docs

PRD 00 should become a short human-readable explanation of the routing concept. The detailed machine-readable map should live in `.context`.

### Step 4: Validate local READMEs

Each work area should have a local README or an explicit reason why it does not yet have one.

### Step 5: Archive stale material

Move non-current material outside the default read path.

### Step 6: Add docs audit gate

Add:

```text
npm run context:docs:audit
```

It should detect:

- active docs that point to stale docs.
- work areas missing local README.
- duplicated work-area tables.
- docs in default read path without current status.

## Proposed final docs shape

```text
docs/
  HANDOFF.md
  current-status.md
  context-transition-status.md
  runbooks/
  specs/
  testing/
  archive/

.context/
  generated/
  curated/
  rules/
  packets/
```

## Agent rule

Agents should use docs for explanation, not for exhaustive file discovery.

Exact file discovery should come from `.context`.
