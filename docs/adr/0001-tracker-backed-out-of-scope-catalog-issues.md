# Use Tracker-Backed Out-of-Scope Catalog Issues

Out-of-scope decisions need durable institutional memory and deduplication, but a local `.out-of-scope/` directory would become a second fact source outside the selected issue tracker. We will represent rejected feature concepts with tracker-backed Out-of-Scope Catalog issues: long-lived issues with canonical type `out-of-scope`, canonical state `cancelled`, and exactly one `## Out-of-Scope Catalog` section containing multiple catalog entries.

Repositories start with one lazily-created `Out-of-Scope Catalog: Overall` issue and split into category-specific catalog issues only when the catalog becomes hard to browse, match, or edit. Catalog entries carry a stable kebab-case slug, short decision, durable reason, optional related-but-allowed concepts, and prior request references with title snapshots.

Ordinary rejected feature requests remain type `feature`; when matched or recorded as out of scope, they enter `state:cancelled`, record `resolution:wontfix`, and are appended to the matching catalog entry's prior requests. Backend tracker closure remains separate from these workflow facts and is performed by the maintainer through the selected tracker backend.

This chooses a small number of special catalog issues over one file per concept, one issue per concept, or scanning all closed `wontfix` issues. The trade-off is an extra canonical issue type and section, but consumers can read a bounded catalog instead of re-searching historical rejected requests.
