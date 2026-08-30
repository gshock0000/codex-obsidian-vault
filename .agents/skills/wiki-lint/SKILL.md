---
name: wiki-lint
description: Audit structure, provenance, freshness, and dependency drift in 06_wiki without silently changing knowledge. Use when the user asks for Wiki health, synchronization, stale-content, or link checks.
---

# Wiki lint

Audit the compiled Wiki and its declared dependencies. Produce a report and a proposed repair set; do not apply semantic repairs without user confirmation.

## Required checks

- Broken internal links, missing required frontmatter, duplicate IDs, and malformed relationship types.
- Missing manifest-issued source IDs or paths, including paths moved to `05_Archived`.
- Local source freshness: continuously select `03_Area` and `04_Resource` files for change detection, then compare current SHA-256 with the fingerprint recorded at last verification. Check `01_inbox` and `02_Project` only when they already have a manifest dependency; do not discover new promotions from them.
- Source metadata completeness for synced Area and Resource notes: require `title`, `type`, `status`, `created`, `updated`, `tags`, `source`, and `summary`, plus `area`. Report missing `status` without inventing a lifecycle value.
- Append-note freshness: detect new content after the latest `wiki-commit` marker and detect edits before that marker.
- Remote source freshness when a stored revision, ETag, or equivalent version is available.
- `wiki/resources` whose direct evidence changed, disappeared, or was archived.
- Transitive impact: concepts, claims, and syntheses supported by a resource that needs review.
- Orphans, unresolved links, contradictions, stale pages, and index drift.

## States and repair boundaries

Use `current`, `needs-review`, `missing-source`, `archived-source`, and `superseded` as appropriate. A stale source marks a derived page for review; it does not prove that the page is false.

Safe structural fixes may be proposed separately from semantic changes. Never overwrite a source note, resource, concept, claim, or synthesis during lint. Keep the manifest as rebuildable verification state and retain historical evidence.
