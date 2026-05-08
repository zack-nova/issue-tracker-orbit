# Tracker Contract

This file is generated or completed during bootstrap. It is the contract-consumer-readable source of truth for the repository's selected issue tracker backend and runtime rule mappings.

If `status` is `pending-bootstrap`, complete `BOOTSTRAP.md` first.

```yaml
version: 1
status: pending-bootstrap
backend: null

repository:
    id: null
    default_branch: main

issue:
    id_format: null
    state:
        required: true
        cardinality: exactly_one
        representation: null
        values: {}
    type:
        required: true
        cardinality: exactly_one
        representation: null
        values: {}
    metadata:
        priority:
            required: false
            representation: null
            values: []
        size:
            required: false
            representation: null
            values: []
        delivery_mode:
            required: false
            representation: null
            values:
                - afk
                - hitl
        resolution:
            required: false
            representation: null
            values:
                - wontfix
                - duplicate

sections:
    triage-notes:
        storage: null
        heading: '## Triage Notes'
    dev-brief:
        storage: null
        heading: '## Dev Brief'
    dev-workpad:
        storage: null
        heading: '## Dev Workpad'
    review-sweep:
        storage: null
        heading: '## Review Sweep'
    human-review-decision:
        storage: null
        heading: '## Human Review Decision'
    debt-notes:
        storage: null
        heading: '## Debt Notes'
    out-of-scope-catalog:
        storage: null
        heading: '## Out-of-Scope Catalog'

review_artifact:
    required: true
    storage: null

validation:
    commands: []
    required_before_review: true
    required_before_land: true

merge:
    method: squash
    delete_branch: true

safety:
    dry_run_required_for:
        - batch_issue_edits
        - batch_metadata_changes
        - template_install_or_overwrite
        - branch_push
        - review_artifact_create
        - merge_or_land
    hard_stops:
        - issue_has_no_state
        - issue_has_multiple_states
        - dev_brief_type_missing_or_mismatched
        - required_section_missing
        - split_state_advancement_without_resolution
        - duplicate_resolution_without_superseding_issue
        - blocked_state_advancement_without_unblock
        - validation_failed_without_waiver
        - review_artifact_missing
        - hitl_review_output_without_human_decision
        - invalid_human_review_decision
        - invalid_delivery_mode
        - hitl_delivery_mode_without_rationale
        - runtime_ownership_modeled_as_issue_fact
```

## Notes

- `backend` is the only backend selector. Do not add a second selector for PR/MR/local review type.
- Do not add `consumers`, `permissions`, or runtime actor role fields. Consumer action authority is defined by the consumer's own orbit, tool, or human process.
- Do not add `backend_mapping`; put machine mapping facts directly under `issue`, `sections`, and `review_artifact`.
- `issue.type` is the source of truth for issue type; the Dev Brief Type line is only a human-readable mirror and uses the canonical type value.
- `issue.metadata.delivery_mode` is optional. When present, it records whether a delivery slice is `afk` or `hitl`; it is not a state role or runtime ownership field.
- `sections` maps canonical issue section storage and headings. Required and optional section semantics are defined by the backend-neutral core.
- Concrete commands, API clients, and execution procedures belong to contract consumers, tools, or human process.
- Contract consumers must read the YAML block before reading explanatory docs.
