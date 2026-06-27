# White Paper: Source Context Management For Agentic Engineering

## Executive summary

Agentic software engineering fails when agents receive either too little context or too much context. Too little context causes missed dependencies, broken tests, and unsafe edits. Too much context causes distraction, slow reasoning, and low precision.

The proposed solution is a source context management layer: a generated, machine-readable map of the codebase combined with a small curated meaning layer and strict routing rules.

The system should produce a context packet for every meaningful task. A context packet tells the solving agent exactly which files to read, in what order, why each file matters, and which tests or checks prove the work.

## Problem

Most code repositories use human documentation as the primary navigation layer. This works poorly for agents because human docs are usually:

- narrative instead of structured.
- duplicated across many files.
- stale relative to source code.
- unclear about which files are required for a task.
- weak at connecting implementation files to tests, configs, entrypoints, and contracts.

Agentic coding systems require a stronger layer. They need a map that can answer operational questions:

```text
What behavior is this task about?
Where does that behavior start?
Which implementation files are on the path?
Which contracts, schemas, configs, and tests are required?
Which files look related but should be excluded?
```

## Proposed solution

Create a `.context` layer inside each agent-operated repository.

Recommended structure:

```text
.context/
  generated/
    files.json
    imports.json
    exports.json
    symbols.json
    tests.json
    entrypoints.json
    graph.json
    checks.json
  curated/
    work-areas.yaml
    behaviors.yaml
    aliases.yaml
  rules/
    relevance-rules.yaml
    risk-rules.yaml
  packets/
    latest-context-packet.example.json
```

The generated layer is produced by an indexer. The curated layer is small and explains meaning the indexer cannot infer reliably. The rules layer defines what counts as relevant for each task type.

## Enterprise pattern

This mirrors how enterprise retrieval and agent systems are commonly designed:

```text
data sources
  -> ingestion/indexing
  -> metadata and permissions
  -> retrieval/routing
  -> selected context
  -> model/agent
  -> logs/evals/feedback
```

For a codebase, the indexed objects are files, symbols, tests, entrypoints, behaviors, modules, configs, and contracts.

## Design principle

Do not ask the solving agent to discover the repository from scratch.

Instead:

```text
index once
retrieve precisely
work safely
record evidence
improve the index when gaps appear
```

## What the context layer must answer

For any task, the system should produce:

- target work area.
- target behavior or responsibility.
- task type.
- exact files to read.
- read order.
- reason for each file.
- tests/checks to run.
- excluded files and why they were excluded.
- confidence score.
- missing context warnings.

## Why this improves quality

This is a quality increase when implemented correctly because it reduces:

- random repository search.
- over-reading.
- stale documentation reliance.
- missed test files.
- wrong-layer edits.
- UI-first changes when backend truth is missing.
- MCP/tool adapter edits when service/domain logic should own the behavior.
- accidental edits to generated/runtime folders.

It also increases:

- repeatability.
- reviewability.
- task isolation.
- file lease precision.
- evidence quality.
- agent handoff quality.
- long-term maintainability.

## What this is not

This is not a replacement for source code.

This is not a giant manual documentation project.

This is not a promise that the indexer can infer human meaning perfectly.

It is a structured operational layer that makes context explicit, queryable, and testable.

## Recommended adoption model

Start additive:

```text
1. Generate source file inventory.
2. Generate import/export/test/entrypoint graph.
3. Add small work-area metadata.
4. Add routing rules.
5. Produce context packets for tasks.
6. Measure missing-file rate.
7. Clean up docs and source areas only where the index shows confusion.
```

Do not begin with broad source restructuring or broad documentation deletion. Use the index to identify what is stale, duplicated, or low-value.

## Success criteria

A context management implementation is successful when a new agent can receive a task and get a packet that answers:

```text
Read these files first.
Do not read these files yet.
Here is why.
Here are the checks.
Here are the risks.
Here is the confidence level.
```

The final goal is not more documentation. The final goal is better agent execution.
