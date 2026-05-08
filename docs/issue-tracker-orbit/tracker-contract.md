# Tracker Contract

This file is generated or completed during bootstrap. It is the contract-consumer-readable source of truth for the repository's selected issue tracker backend and runtime rule mappings.

If `status` is `pending-bootstrap`, complete `BOOTSTRAP.md` first.

```yaml
version: 1
status: active
backend: github

repository:
    id: zack-nova/issue-tracker-orbit
    default_branch: main

issue:
    id_format: "#<number>"
    url_format: "https://github.com/zack-nova/issue-tracker-orbit/issues/<number>"
    state:
        required: true
        cardinality: exactly_one
        representation: label
        prefix: "state:"
        values:
            needs-triage: "state:needs-triage"
            needs-info: "state:needs-info"
            needs-split: "state:needs-split"
            blocked: "state:blocked"
            ready-for-dev: "state:ready-for-dev"
            in-progress: "state:in-progress"
            in-review: "state:in-review"
            human-review: "state:human-review"
            to-rework: "state:to-rework"
            to-merge: "state:to-merge"
            merged: "state:merged"
            duplicate: "state:duplicate"
            cancelled: "state:cancelled"
    type:
        required: true
        cardinality: exactly_one
        representation: label
        prefix: "type:"
        values:
            bug: "type:bug"
            feature: "type:feature"
            task: "type:task"
            docs: "type:docs"
            chore: "type:chore"
    metadata:
        priority:
            required: false
            representation: label
            prefix: "priority:"
            values: []
        size:
            required: false
            representation: label
            prefix: "size:"
            values: []
        resolution:
            required: false
            representation: label
            prefix: "resolution:"
            values: []

sections:
    triage-notes:
        storage: issue_comment
        heading: '## Triage Notes'
    dev-brief:
        storage: issue_comment
        heading: '## Dev Brief'
    dev-workpad:
        storage: issue_comment
        heading: '## Dev Workpad'
    review-sweep:
        storage: issue_comment
        heading: '## Review Sweep'
    human-review-decision:
        storage: issue_comment
        heading: '## Human Review Decision'
    debt-notes:
        storage: issue_comment
        heading: '## Debt Notes'

review_artifact:
    required: true
    storage: github_pull_request
    link_rule: closing_reference

validation:
    commands: []
    required_before_review: false
    required_before_land: false
    waiver: "No automatic validation command is configured for this documentation-only orbit repository."

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
        - blocked_state_advancement_without_unblock
        - validation_failed_without_waiver
        - review_artifact_missing
        - review_output_without_human_decision
        - invalid_human_review_decision
        - runtime_ownership_modeled_as_issue_fact
```

## Notes

- `backend` is the only backend selector. Do not add a second selector for PR/MR/local review type.
- Do not add `consumers`, `permissions`, or runtime actor role fields. Consumer action authority is defined by the consumer's own orbit, tool, or human process.
- Do not add `backend_mapping`; put machine mapping facts directly under `issue`, `sections`, and `review_artifact`.
- `issue.type` is the source of truth for issue type; the Dev Brief Type line is only a human-readable mirror and uses the canonical type value.
- `sections` maps canonical issue section storage and headings. Required and optional section semantics are defined by the backend-neutral core.
- Concrete commands, API clients, and execution procedures belong to contract consumers, tools, or human process.
- Contract consumers must read the YAML block before reading explanatory docs.
