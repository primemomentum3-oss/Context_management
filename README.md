# Context Management

This repository defines the proposed enterprise-grade context management layer for agentic software engineering systems.

The purpose is not to create human-only documentation. The purpose is to define an agent-usable system that can answer:

```text
Given this task, what exact files, tests, contracts, configs, and docs must the solving agent read first?
```

## Core idea

A mature agentic coding system should not depend on agents randomly searching a repository. It should maintain a generated, machine-readable source context index.

```text
codebase
  -> indexer
  -> .context/generated/*
  -> curated meaning layer
  -> relevance rules
  -> routing agent
  -> context packet
  -> solving agent
```

## Deliverables in this repository

| Path | Purpose |
| --- | --- |
| `docs/00-white-paper.md` | Strategic white paper explaining why this context layer exists and how it improves agent quality. |
| `docs/01-context-management-standard.md` | The technical standard for `.context`, indexer outputs, curated metadata, relevance rules, and context packets. |
| `docs/02-agent-os-critical-review.md` | Critical review of the current Agent OS documentation/system posture against this standard. |
| `docs/03-agent-os-implementation-plan.md` | Practical implementation plan for adding this to Agent OS. |
| `docs/04-agent-os-documentation-redesign.md` | Proposed AI-suitable documentation redesign and cleanup model. |
| `docs/05-indexer-technical-spec.md` | Technical design for the source context indexer. |
| `docs/06-routing-agent-contract.md` | Input/output contract for the routing agent. |
| `docs/07-context-packet-contract.md` | Contract for the packet passed from routing agent to solving agent. |
| `docs/08-quality-gates-and-maintenance.md` | Validation rules, CI checks, quality gates, and ongoing maintenance model. |
| `agent-instructions/` | Task-ready instruction packets for implementation agents. |
| `schemas/` | JSON schemas for generated index and context packet objects. |
| `examples/` | Example work-area and context-packet structures. |

## Recommended use

1. Read `docs/00-white-paper.md` for the reasoning.
2. Use `docs/01-context-management-standard.md` as the source of truth.
3. For Agent OS, start with `docs/02-agent-os-critical-review.md` and `docs/03-agent-os-implementation-plan.md`.
4. Give the implementation packets in `agent-instructions/` to coding agents.
5. Use the schemas in `schemas/` to implement generated `.context` files.

## Non-goals

This repository does not require rewriting a codebase before context management can begin. The first version should be additive:

```text
add indexer
add generated context
add small curated metadata
add routing contract
use results to identify cleanup targets
```

Large documentation cleanup and source restructuring should be performed only after the index exposes where the current system is confusing, stale, duplicated, or risky.
