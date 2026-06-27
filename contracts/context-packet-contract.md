# Context Packet Contract Summary

Required top-level fields:

- schema_version
- generated_at
- repo
- task
- routing
- read_order
- checks
- excluded
- warnings

Task fields:

- id
- title
- user_request
- task_type

Routing fields:

- target_work_area
- target_behavior
- confidence
- index_version

Read-order item fields:

- path
- kind
- role
- reason
- required

Check item fields:

- command
- type
- reason
- requires_disposable_data

Excluded item fields:

- path
- reason

Warning item fields:

- level
- message

A valid context packet must answer:

```text
what to read
why to read it
what to verify
what to avoid
how confident the router is
```
