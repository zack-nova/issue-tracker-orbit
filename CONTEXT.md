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

**Tracker Contract**:
The contract-consumer-readable source of truth for a repository's selected issue tracker backend and runtime rule mappings.
_Avoid_: Tracker config, workflow config

**Contract Consumer**:
An external agent, runtime participant, or tool that must read and obey the **Tracker Contract** before participating in issue workflow.
_Avoid_: Consumer, worker, integration

## Relationships

- A **Backend Template** belongs to exactly one backend.
- An **Issue Section Template** can be used by multiple backends.
- A **Template Target** is selected through the active tracker contract.
- A **Contract Consumer** reads exactly one active **Tracker Contract** for a target repository before acting on managed issues.
- A **Tracker Contract** selects exactly one backend.

## Example Dialogue

> **Dev:** "Should this issue form live under the GitHub templates?"
> **Domain expert:** "It is a **Backend Template**, so keep it under `templates/backends/github/`; the **Template Target** is still `.github/ISSUE_TEMPLATE/`."

## Flagged Ambiguities

- "GitHub template" was used for both the source template group and the real `.github/` install target; resolved by using **Backend Template** for the source and **Template Target** for the install location.
