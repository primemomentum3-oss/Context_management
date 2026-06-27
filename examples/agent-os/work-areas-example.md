# Agent OS Work Areas Example

This file shows how broad Agent OS areas can be represented in structured context.

## workflow_harness

Name: Workflow / Harness

Plain meaning: Backend traffic controller for work lifecycle.

Aliases:

- workflow
- harness
- lifecycle
- ready for approval

Primary paths:

- `src/domain/workflowStage.ts`
- `src/domain/reliabilityGates.ts`
- `src/services/workflow.ts`
- `src/services/workflow/`
- `src/services/automaticExecutor.ts`

Checks:

- `tests/workflowStage.test.ts`
- `npm run workflow:smoke`
- `npm run verification-gates:smoke`

Owns:

- workflow stage calculation
- readiness checks
- fix-pass routing

Does not own:

- UI-only statuses
- provider-specific launch behavior

Boundary rule:

- backend records decide lifecycle state.

## evidence_artifacts_trace

Name: Evidence / Artifacts / Trace

Plain meaning: Proof of what happened and why a result should be trusted.

Primary paths:

- `src/services/evidence/`
- `src/services/commands.ts`
- `src/services/patches.ts`
- `src/services/verificationGates.ts`
- `src/services/reports.ts`

Checks:

- `npm run verification-gates:smoke`
- `npm run workflow:smoke`

Owns:

- command evidence
- patch artifacts
- verification evidence
- report evidence summaries

Boundary rule:

- reports summarize evidence; they do not replace evidence.
