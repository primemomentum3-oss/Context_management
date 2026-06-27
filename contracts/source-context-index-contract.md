# Source Context Index Contract

Required top-level fields:

- schema_version
- generated_at
- repo
- files
- work_areas
- warnings

Required file-card fields:

- path
- kind
- language
- work_area
- roles
- exports
- imports
- imported_by
- entrypoint
- tests
- configs
- contracts
- generated
- runtime
- warnings

Required work-area fields:

- id
- name
- plain_meaning
- aliases
- primary_paths
- checks
- owns
- does_not_own
- boundary_rules

Example file card:

```text
path: src/services/workflow.ts
kind: source
language: typescript
work_area: workflow_harness
roles: service_logic
entrypoint: false
generated: false
runtime: false
```
