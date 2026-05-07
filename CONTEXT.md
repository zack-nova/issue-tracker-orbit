# Issue Tracker Contract Orbit

This context defines the language for describing issue tracker contracts and their portable documentation assets.

## Language

**Tracker Backend**:
The concrete issue tracker selected during bootstrap for one repository.
_Avoid_: default issue store, template backend

**Tracker Contract**:
The generated repository-local contract that records the selected **Tracker Backend** and the rule mapping contract consumers must follow for that repository.
_Avoid_: adapter template, docs index, platform guess

**Contract Block**:
A machine-friendly YAML block inside the **Tracker Contract** that contract consumers read before explanatory prose.
_Avoid_: prose-only contract, inferred rules

**Human Review Decision**:
The human-recorded post-review decision that authorizes hold, rework, or merge workflow movement.
_Avoid_: review sweep output, placeholder decision

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
- A **Tracker Contract** contains exactly one **Contract Block**.
- A **Tracker Backend** is selected by evidence during bootstrap before backend templates are installed.
- A **Human Review Decision** exists only after a human records exactly one decision value.

## Example Dialogue

> **Dev:** "Should this issue form live under the GitHub templates?"
> **Domain expert:** "It is a **Backend Template**, so keep it under `templates/backends/github/`; the **Template Target** is still `.github/ISSUE_TEMPLATE/`."

## Flagged Ambiguities

- "GitHub template" was used for both the source template group and the real `.github/` install target; resolved by using **Backend Template** for the source and **Template Target** for the install location.
- "`Decision: hold | rework | merge` in a new issue template looked like a valid **Human Review Decision** to a machine parser; resolved: new issue and review artifact templates must not create a parseable decision field until a human records exactly one decision value.
- "`backend_mapping` repeated top-level issue, section, review artifact, and template facts inside the **Tracker Contract**; resolved: the **Contract Block** uses `issue`, `sections`, `review_artifact`, and `templates` as the only machine fact fields, while **Backend Templates** remain bootstrap inputs.
- "default tracker backend" could make local markdown or a manifest default override a real project's hosted tracker; resolved: backend bootstrap uses automatic evidence-based selection and prefers existing GitHub or GitLab tracker evidence over local markdown templates.
