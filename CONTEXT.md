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

**Out-of-Scope Catalog Issue**:
A long-lived issue that stores multiple rejected feature concepts for deduplication and institutional memory.
_Avoid_: Out-of-scope file, rejected issue index

**Out-of-Scope Catalog Entry**:
A single rejected feature concept with a stable kebab-case slug recorded inside an Out-of-Scope Catalog Issue.
_Avoid_: Out-of-scope issue, rejected request

**Out-of-Scope Catalog Section**:
The canonical issue section that stores out-of-scope catalog entries.
_Avoid_: Dev Brief, Triage Notes

**Out-of-Scope Decision**:
A maintainer decision that a feature request is outside the project's accepted scope.
_Avoid_: Agent rejection, similarity match

**Tracker Closure**:
The backend-specific action that removes an issue from the active open set.
_Avoid_: Cancelled state, resolution

**Duplicate Resolution**:
A cancellation resolution used when an issue is superseded by another managed issue and should not advance independently.
_Avoid_: Duplicate state, duplicate issue database

**Delivery Slice**:
A managed delivery issue small enough to be implemented, reviewed, and landed as one unit.
_Avoid_: Runtime session, worktree, actor assignment

**Delivery Mode**:
Optional issue metadata that classifies a Delivery Slice as `afk` or `hitl`.
_Avoid_: State role, ownership field, queue claim

**AFK Slice**:
A Delivery Slice that can be implemented, reviewed, and landed without human interaction when objective repository gates pass.
_Avoid_: Unreviewed work, lower-quality work

**HITL Slice**:
A Delivery Slice that needs human interaction because safe advancement depends on judgment calls, external access, design decisions, architecture decisions, or manual testing.
_Avoid_: Blocked issue, human-owned issue

## Relationships

- A **Backend Template** belongs to exactly one backend.
- An **Issue Section Template** can be used by multiple backends.
- A **Template Target** is selected through the active tracker contract.
- An **Out-of-Scope Catalog Issue** contains one or more **Out-of-Scope Catalog Entries**.
- A repository starts with one general **Out-of-Scope Catalog Issue** named `Out-of-Scope Catalog: Overall` and splits into category-specific catalog issues only when maintenance pressure appears.
- The first **Out-of-Scope Catalog Issue** is created lazily when a maintainer enables the catalog or the first **Out-of-Scope Decision** needs to be recorded.
- An **Out-of-Scope Catalog Issue** title uses the human-readable prefix `Out-of-Scope Catalog:`.
- An **Out-of-Scope Catalog Issue** is a terminal workflow record with canonical state `cancelled` and canonical issue type `out-of-scope`.
- Contract consumers discover **Out-of-Scope Catalog Issues** through canonical issue type `out-of-scope`, not through title text.
- Canonical issue type `out-of-scope` must always pair with canonical state `cancelled`.
- An **Out-of-Scope Catalog Issue** requires one **Out-of-Scope Catalog Section** instead of delivery sections such as Dev Brief, Dev Workpad, Review Sweep, and Human Review Decision.
- Categories are represented by separate **Out-of-Scope Catalog Issues**, not multiple Out-of-Scope Catalog sections in one issue.
- A rejected feature request remains type `feature`, enters terminal state `cancelled`, records resolution `wontfix`, and is linked from a matching **Out-of-Scope Catalog Entry**.
- A superseded issue keeps its original issue type, enters terminal state `cancelled`, records **Duplicate Resolution** as `resolution:duplicate`, and references the superseding issue.
- A new **Out-of-Scope Catalog Entry** is created only after an **Out-of-Scope Decision**.
- Splitting catalog issues moves **Out-of-Scope Catalog Entries** without changing their stable slugs or prior request references.
- Prior requests in an **Out-of-Scope Catalog Entry** use an issue reference plus a title snapshot.
- `state:cancelled`, `resolution:wontfix`, and `resolution:duplicate` are issue facts; **Tracker Closure** is a separate maintainer-owned operation.
- GitHub and GitLab perform **Tracker Closure** with the platform close action; local markdown performs it by moving the issue file from an open folder to a closed folder.
- A contract consumer may record an out-of-scope match, update state and resolution facts, and append prior requests, but must not perform **Tracker Closure**.
- A **Delivery Slice** may omit **Delivery Mode** when the repository or issue does not need that classification.
- When **Delivery Mode** is recorded, it has exactly one value: `afk` or `hitl`.
- **AFK Slice** is the preferred classification when the work can safely pass through implementation, review, and land using objective repository gates.
- **HITL Slice** requires a recorded note explaining why the slice cannot be delegated without human interaction.
- `human-review` is an optional state for **HITL Slices** or for **AFK Slices** that a contract consumer escalates because review evidence shows human judgment is needed.
- `human-review` is not a mandatory state for every **Delivery Slice** after `in-review`.

## Example Dialogue

> **Dev:** "Should this issue form live under the GitHub templates?"
> **Domain expert:** "It is a **Backend Template**, so keep it under `templates/backends/github/`; the **Template Target** is still `.github/ISSUE_TEMPLATE/`."
>
> **Dev:** "Should every rejected feature get its own issue?"
> **Domain expert:** "No — create or update an **Out-of-Scope Catalog Entry** inside the general **Out-of-Scope Catalog Issue** first, then split the catalog only when it becomes too large."
>
> **Dev:** "Why is the catalog issue closed if triage still reads it?"
> **Domain expert:** "Because an **Out-of-Scope Catalog Issue** is not pending work; its `out-of-scope` type makes it discoverable as a durable reference even after **Tracker Closure**."
>
> **Dev:** "Should the catalog issue have a Dev Brief?"
> **Domain expert:** "No — its **Out-of-Scope Catalog Section** is the durable record; delivery sections only apply to normal work issues."
>
> **Dev:** "Do we change the rejected request itself to `type:out-of-scope`?"
> **Domain expert:** "No — the request stays `feature`; only the catalog issue uses `type:out-of-scope`."
>
> **Dev:** "Can the agent create a new catalog entry when it thinks a feature should not be built?"
> **Domain expert:** "No — a new **Out-of-Scope Catalog Entry** needs an **Out-of-Scope Decision** first."
>
> **Dev:** "When should we split `Out-of-Scope Catalog: Overall`?"
> **Domain expert:** "Only when the catalog becomes hard to browse, match, or edit; entry slugs stay stable when moved."
>
> **Dev:** "Should every review go through `human-review`?"
> **Domain expert:** "No — an **AFK Slice** can skip `human-review` when objective review gates pass. A **HITL Slice** enters `human-review` and records why human interaction is required."
>
> **Dev:** "Is `hitl` the same as assigning the issue to a person?"
> **Domain expert:** "No — **Delivery Mode** is optional metadata about delegation safety, not runtime ownership or queue state."

## Flagged Ambiguities

- "GitHub template" was used for both the source template group and the real `.github/` install target; resolved by using **Backend Template** for the source and **Template Target** for the install location.
- New issue templates exposed `Decision: hold | rework | merge` as parseable placeholder text; resolved: templates do not create decision fields until a human writes one value.
- `backend_mapping` duplicated contract facts; resolved: `issue`, `sections`, and `review_artifact` are the machine mapping fields, with backend-specific fields added only when populated.
- Local markdown templates looked like a default backend; resolved: bootstrap selects backend from project evidence and prefers existing GitHub/GitLab trackers.
- "Out-of-scope knowledge base" implied one local file per rejected concept; resolved: tracker-backed catalogs use one general **Out-of-Scope Catalog Issue** at first, with multiple **Out-of-Scope Catalog Entries** inside it.
- "Open issue" was considered for catalog discoverability; resolved: **Out-of-Scope Catalog Issues** are closed terminal records and stay discoverable through canonical issue type `out-of-scope`.
- "Catalog" was considered as a metadata marker; resolved: catalog names classify long-lived directory issues, while canonical issue type `out-of-scope` identifies their workflow role.
- "Required issue sections" originally implied every managed issue needed delivery sections; resolved: **Out-of-Scope Catalog Issues** require an **Out-of-Scope Catalog Section** instead.
- "Closed issue" was used for both terminal workflow state and backend closure; resolved: use `state:cancelled` for workflow termination and **Tracker Closure** for the platform close action or local markdown file move.
- "Duplicate issue" was used as a terminal workflow state; resolved: use `state:cancelled` with **Duplicate Resolution** and a superseding issue reference.
- "Slice" was introduced as a delivery unit; resolved: use **Delivery Slice** when the unit is a managed delivery issue, and do not use it for runtime sessions or worktrees.
- "Human review" was treated as a required post-review state; resolved: `human-review` is optional and used only for **HITL Slices** or when an **AFK Slice** is escalated by review evidence.
