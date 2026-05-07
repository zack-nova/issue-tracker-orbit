# Other Backend Adapter

Use this adapter only when the repository uses an issue tracker not covered by GitHub, GitLab, or local markdown.

Bootstrap must write the selected backend's concrete facts directly into the `tracker-contract.md` contract block. Freeform prose is not enough, and a separate `backend_mapping` block would create a second machine source of truth.

## Required Mapping

The generated tracker contract must specify:

- how to identify an issue,
- where state is stored,
- where issue type is stored,
- how optional metadata is stored,
- where each canonical issue section is stored,
- how the review artifact is identified,
- where review and land evidence is recorded,
- how validation evidence is recorded,
- which workflow changes require dry-run,
- which states or conflicts are hard stops.

If any required mapping is unclear, stop bootstrap and ask a human maintainer.
