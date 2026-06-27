# Agent OS Critical Review Against The Context Management Standard

## Review scope

This review evaluates Agent OS as an example target system for source context management.

The review is based on the available repository entrypoints, including:

- `README.md`
- `docs/HANDOFF.md`
- `docs/current-status.md`
- `docs/prds/README.md`
- `docs/prds/00-agent-os-work-area-index-and-navigation-prd.md`
- `docs/agent/work-area-router.md`
- representative local README files such as `src/services/workflow/README.md`, `src/mcp/README.md`, and `src/services/evidence/README.md`
- `package.json`

This review is intentionally non-biased. It identifies what is already strong and what must change to support a true machine-readable context layer.

## Executive verdict

Agent OS is directionally aligned with source context management, but it is not yet implementing source context management as a first-class system.

Current state:

```text
strong human-readable navigation
partial local area README model
strong backend evidence/workflow orientation
no generated source context index
no context packet file-selection engine
no measurable missing-context feedback loop
```

Recommended verdict:

```text
Adopting .context-style context management is a quality increase for Agent OS.
```

Reason: Agent OS is already built to coordinate agents, evidence, runs, reviews, and approval. It lacks the same level of structure for selecting the exact files an agent should read before work begins.

## What Agent OS already does well

### 1. It already recognizes that agents need navigation

Agent OS already has a Work Area Index and a Work Area Router. This is the correct product intuition.

The current routing model is:

```text
Stig phrase -> stable work area -> local area README -> source/tests/checks
```

This is a strong foundation.

### 2. It already separates broad work areas

The repo already distinguishes areas such as:

- UI / Operator Workbench
- Projects / Tasks / Worktrees
- Intake / Work Classification
- Workflow / Harness
- Sessions / Runs / Launchers
- MCP / Tool Gateway
- Hooks / Automatic Capture
- Evidence / Artifacts / Trace
- Validation / Review / Approval
- Memory / Learning / Evals
- Database
- CLI / Runbooks / Scripts

This maps well to `.context/curated/work-areas.yaml`.

### 3. It already has local README patterns

The local README files are useful because they define:

- owns
- does not own
- main entrypoints
- invariants
- tests
- common mistakes

This is exactly the kind of material that should be converted into structured context metadata.

### 4. It already has strong lifecycle/evidence concepts

Agent OS is not only a coding assistant wrapper. It records tasks, worktrees, leases, evidence, verification, reviews, closer packages, and memory candidates. This makes it a good candidate for context-packet integration.

## Main weaknesses

### 1. Documentation is still the context system

The current context model depends heavily on Markdown reading paths. Markdown is useful for humans, but weak as an agent routing layer because it is:

- not typed.
- not automatically validated against source files.
- not easy to query precisely.
- not guaranteed to remain current when source files move.
- not directly usable for file leases and task packets.

### 2. PRD 00 is doing too much

PRD 00 is valuable, but it is very large. It is a transition work-area map, routing guide, quality standard, gap tracker, restructure guide, and risk guide in one file.

This creates a risk that agents treat it as a global source of truth for everything.

Better structure:

```text
PRD 00 = human-readable broad routing and product intent
.context/curated/work-areas.yaml = machine-readable work area map
.context/generated/graph.json = generated source graph
.context/rules/*.yaml = routing and maintenance rules
```

### 3. There is no generated source index

Agent OS currently tells agents where to start. It does not yet generate a source map that can answer:

```text
which exact files are relevant to this task?
which tests verify those files?
which entrypoints reach this behavior?
which files are direct callers/dependencies?
which files are generated/runtime and must be excluded?
```

### 4. There is no formal context packet contract

Agent OS has context packets in the runtime direction, but it does not yet have a strict source-context packet that includes:

- exact read order.
- file roles.
- inclusion reasons.
- excluded files.
- checks.
- confidence.
- missing context warnings.

### 5. Docs cleanup should not be manual-first

The user perception that docs are messy is likely valid because the documentation layer contains current docs, historical docs, PRDs, runbooks, specs, logs, and archived material.

However, broad deletion first would be risky.

The correct approach is:

```text
index docs -> classify docs -> mark source-of-truth vs support vs historical -> migrate current context into .context -> archive or remove stale material after validation
```

### 6. File-level coverage is not yet enforceable

Agent OS has area-level routing, but not guaranteed file-level coverage. A context indexer should enforce:

- every source file belongs to a work area.
- every entrypoint has an owner.
- every important behavior has checks.
- every generated/runtime path is excluded from source routing.

## Quality increase assessment

Adopting this context layer would be a quality increase if implemented as generated + curated + validated.

Estimated improvement areas:

| Area | Expected improvement |
| --- | --- |
| Agent onboarding | High |
| File selection precision | High |
| Documentation cleanup | High |
| Worktree/file lease scoping | High |
| Test selection | Medium-high |
| Human readability | Medium |
| Runtime safety | Medium-high |
| Implementation speed | Medium |

## Risks

### Risk 1: Creating a second documentation mess

If `.context` is manually maintained like normal docs, it will fail.

Mitigation:

```text
90% generated
10% curated
validated by CI
```

### Risk 2: Over-indexing too early

Trying to build a perfect semantic graph before using anything can stall implementation.

Mitigation:

Start with practical index outputs:

- files
- imports
- tests
- entrypoints
- checks
- work areas
- generated/runtime exclusions

### Risk 3: Deleting useful historical docs too soon

Old notes may still contain decision history.

Mitigation:

Archive first, delete only after explicit classification and confirmation.

### Risk 4: Context confidence being treated as truth

The index can be wrong.

Mitigation:

Every packet includes confidence and missing-context reporting.

## Recommended conclusion

Do not replace Agent OS docs with `.context` in one step.

Instead:

```text
1. Add .context.
2. Generate source/docs index.
3. Convert current work-area router into curated machine-readable work areas.
4. Make context packets part of task creation.
5. Use packet quality to drive docs cleanup.
6. Remove or archive stale docs only after classification.
```

This would move Agent OS from human-readable navigation to agent-operational context management.
