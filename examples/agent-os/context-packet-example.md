# Agent OS Context Packet Example

Task:

```text
The workflow says ready for approval even when verification failed.
```

Routing result:

```text
task_type: bug_fix
target_work_area: workflow_harness
target_behavior: workflow_ready_for_approval
confidence: 0.84
```

Read order:

1. `src/domain/workflowStage.ts`
   - Role: domain rule
   - Reason: calculates workflow stage and readiness.

2. `src/domain/reliabilityGates.ts`
   - Role: domain rule
   - Reason: defines verification and reliability gate rules.

3. `src/services/workflow/closerPackages.ts`
   - Role: service logic
   - Reason: closer package readiness depends on verification and review state.

4. `src/services/verificationGates.ts`
   - Role: verification service
   - Reason: stores and evaluates verification evidence.

5. `tests/workflowStage.test.ts`
   - Role: test
   - Reason: verifies workflow stage behavior.

6. `tests/reliabilityGates.test.ts`
   - Role: test
   - Reason: verifies gate logic.

Checks:

- `npm run workflow:smoke`
- `npm run verification-gates:smoke`

Excluded first-pass paths:

- `apps/desktop/`
  - Reason: UI displays status but should not own lifecycle truth.

- `src/mcp/`
  - Reason: tool gateway should not own workflow readiness logic.

Warnings:

- If workflow and validation areas both match, start with backend workflow rules and then inspect validation/review approval code if needed.
