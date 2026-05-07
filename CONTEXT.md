# Issue Tracker Contract Orbit

This context defines the language for describing issue tracker contracts and their portable documentation assets.

## Language

**Backend Template**:
A reference template that is specific to one selected issue tracker backend and is installed or merged during bootstrap.
_Avoid_: GitHub template, platform template

**Issue Section Template**:
A backend-neutral reference template for a canonical issue section.
_Avoid_: Backend template

**Template Target**:
The repository location or tracker location where a bootstrap step installs or merges a template.
_Avoid_: Source template

## Relationships

- A **Backend Template** belongs to exactly one backend.
- An **Issue Section Template** can be used by multiple backends.
- A **Template Target** is selected through the active tracker contract.

## Example Dialogue

> **Dev:** "Should this issue form live under the GitHub templates?"
> **Domain expert:** "It is a **Backend Template**, so keep it under `templates/backends/github/`; the **Template Target** is still `.github/ISSUE_TEMPLATE/`."

## Flagged Ambiguities

- "GitHub template" was used for both the source template group and the real `.github/` install target; resolved by using **Backend Template** for the source and **Template Target** for the install location.
- New issue templates exposed `Decision: hold | rework | merge` as parseable placeholder text; resolved: templates do not create decision fields until a human writes one value.
- `backend_mapping` duplicated contract facts; resolved: `issue`, `sections`, `review_artifact`, and `templates` are the machine mapping fields.
- Local markdown templates looked like a default backend; resolved: bootstrap selects backend from project evidence and prefers existing GitHub/GitLab trackers.
