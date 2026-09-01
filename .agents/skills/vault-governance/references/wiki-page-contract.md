# Wiki page contract

This is the canonical V1 contract for durable knowledge pages under
`06_wiki/resources`, `concepts`, `entities`, `claims`, and `syntheses`.
`index.md`, `overview.md`, commit logs, and `.state/` have separate contracts.

## Page and file identity

- Give every knowledge page a stable ID matching
  `wiki-(resource|concept|entity|claim|synthesis)-[a-z0-9]+(-[a-z0-9]+)*`.
  The ID does not change when the title or path changes.
- Keep V1 knowledge pages directly inside their type folder. Use tags,
  relationships, and navigation for subject organization; adding nested Wiki
  taxonomy is a schema migration, not an ingest side effect.
- Use a descriptive filename. Unicode filenames are allowed, but every basename
  must be unique by Unicode-NFC casefold across `06_wiki` so Obsidian short
  links cannot resolve to the wrong page.
- Use vault-root-qualified wikilinks in prose, for example
  `[[06_wiki/concepts/knowledge-commit|Knowledge commit]]`. Do not rely on
  Obsidian's shortest-path setting or the current page directory.
- Resolve relationships by stable page ID, not filename. Never create an empty
  page merely to satisfy a link.

## Canonical frontmatter

All knowledge pages use this common shape:

```yaml
---
id: wiki-concept-knowledge-commit
type: concept
title: "Knowledge commit"
status: accepted
confidence: high
created: 2026-09-01
updated: 2026-09-01
summary: "A user-approved, provenance-preserving semantic change to the compiled Wiki."
tags: [knowledge-management]
aliases: []
origins:
  projects: []
  areas: []
evidence_refs:
  - wiki-resource-example-evidence
relations:
  - type: derived_from
    target_id: wiki-resource-example-evidence
last_confirmed_commit: kc-20260901T120000Z-a1b2c3
---
```

Required on every page: `id`, `type`, `title`, `status`, `confidence`,
`created`, `updated`, `summary`, `tags`, `origins`, `relations`, and
`last_confirmed_commit`. `aliases` is optional and defaults to an empty list.

Field rules:

- `type` must match the containing folder: `resource`, `concept`, `entity`,
  `claim`, or `synthesis`.
- `status` records the last user-confirmed semantic lifecycle. Allowed values
  are `provisional`, `accepted`, `disputed`, `superseded`, and `archived`.
  Source drift never changes this field by itself.
- `confidence` is `low`, `medium`, or `high`. Base it on evidence completeness,
  source quality, independence, and unresolved contradiction—not model
  confidence or a numeric formula. Explain the basis in the body.
- `created` and `updated` are ISO dates. Update `updated` only when the page's
  human-readable content or semantic metadata changes.
- `summary` is retrieval text, not a second body. Keep it at 200 characters or
  fewer, keep it on one line, and make it understandable without opening the
  page. Keep `title` on one line as well; neither field may contain the Wiki-link
  delimiters `|` or `]]`, which would make deterministic index entries
  ambiguous.
- `tags` describe subjects. Do not encode page type or PARA ownership as tags.
- `origins.projects` and `origins.areas` name contexts that materially shaped
  the page. They are not evidence references and may be empty.
- `last_confirmed_commit` names the commit that last changed the page. It must
  resolve to exactly one file under `06_wiki/log/`.
- When `status: superseded`, add `superseded_by: <new-page-id>`. The successor
  carries a `supersedes` relationship back to the older page.

Do not put content hashes, mtimes, ETags, computed review state, or mutable
dependency paths in common semantic fields. Those belong in `.state/manifest.json`.

## Evidence fields

Resource pages use `source_refs` instead of `evidence_refs`:

```yaml
source_refs:
  - source_id: src-a1b2c3d4e5f6
    locator: "03_Area/rf/antenna-notes.md#Matching trade-offs"
    evidence_kind: area-note
```

- `source_id` is assigned once by the manifest and remains stable.
- `locator` is a human-readable, vault-relative POSIX path plus optional heading,
  or an HTTPS URL. It is a locator at the time of the last confirmed commit;
  the manifest holds the current locator and its history.
- `evidence_kind` is a short lowercase label such as `raw`, `research`,
  `decision`, `confirmation`, `test`, `activity`, `knowledge-card`, `area-note`,
  `resource-card`, or `remote`. Lint warns on unfamiliar labels rather than
  inventing a category.

Concept, entity, claim, and synthesis pages use `evidence_refs`, a list of Wiki
resource page IDs only. Every material conclusion must cite at least one
resource directly. Link other concepts, entities, claims, or syntheses through
typed relationships and prose, not as a substitute for evidence. Do not cite an
Inbox, Project, Area, or Resource source directly from a derived page to bypass
its resource page.

## Relationship vocabulary

Relationships are directed and use only these types:

| Type | Direction and meaning |
| --- | --- |
| `derived_from` | This page depends on the target evidence or antecedent. |
| `supports` | This page supplies positive evidence for the target proposition. |
| `refines` | This page narrows, qualifies, or makes the target more precise. |
| `contradicts` | This page supplies evidence or a conclusion incompatible with the target in a stated scope. |
| `applies_to` | This knowledge is applicable to the target entity or context. |
| `uses` | This page relies on the target as a method, component, or knowledge dependency. |
| `supersedes` | This newer artifact replaces the target for future use without deleting it. |

Each relation object has exactly `type` and `target_id`. Explain a non-obvious
edge in prose and include a path-qualified wikilink to the target. Do not create
automatic reciprocal edges or add links to meet a quota. A contradiction is
preserved and scoped; it is never silently converted into a refinement.

The semantic relationship graph is not identical to the freshness dependency
graph. `evidence_refs` always create page-to-resource dependencies.
`derived_from` and `uses` also create page-to-target dependencies. `supports`
and `contradicts` point from evidence/knowledge toward the proposition they
assess; `refines`, `applies_to`, and `supersedes` express meaning but do not by
themselves create a freshness dependency. Add `derived_from` explicitly when a
page must be reviewed after such a target changes.

## Minimum body by page type

### Resource

Use `Summary`, `Evidence`, `Compiled understanding`, `Contradictions and
limits`, and `Connections`. In `Evidence`, tie each material observation to a
`source_id` and exact heading, page, table, test, or data locator. Separate
source facts from agent inference.

### Concept

Use `Definition and scope`, `Explanation`, `Evidence`, `Boundaries and
counterexamples`, `Open questions`, and `Related pages`. A method or reusable
pattern is a concept; do not create a separate top-level type merely for it.

### Entity

Use `Identity and scope`, `Evidence-backed facts`, `Relationships`,
`Uncertainties`, and `Related pages`. Create an entity only when it anchors
reusable knowledge; do not create a stub for every mentioned noun.

### Claim

Use `Claim`, `Scope and conditions`, `Supporting evidence`, `Contradicting
evidence`, `Assessment`, and `Related pages`. The claim must be falsifiable or
decision-relevant. Reserve claim pages for high-value assertions; ordinary
statements remain inside resources or concepts.

### Synthesis

Add `as_of: YYYY-MM-DD` to frontmatter. Use `Question and scope`, `Evidence`,
`Analysis`, `Strongest objection`, `Recommendations`, `Open questions`, and
`Related pages`. Label inference explicitly. When later evidence changes the
conclusion materially, create a successor rather than erasing the dated view.

## Navigation files

- `index.md` is a thin deterministic catalog, not a knowledge page. Start with
  `<!-- generated-from: wiki-page-frontmatter -->` and `# Wiki index`. Use
  section order `Resources`, `Concepts`, `Entities`, `Claims`, `Syntheses`, then
  `History`. Put `accepted`, `provisional`, and `disputed` pages in their type
  section; put every `archived` or `superseded` page in `History`. List every
  page exactly once as
  `- [[06_wiki/<folder>/<file>|<title>]] — <summary> _(status; confidence)_`.
  Sort within a section by the code-point sequence of
  `Unicode-NFC(title).casefold()`, then stable ID; never use locale collation.
- `overview.md` is a selective, human-readable view of the Wiki's durable
  picture. Create or update it only when that picture changes, and include the
  change in a confirmed knowledge-commit proposal.
- Do not create either file as a placeholder. The first confirmed commit that
  needs one creates it.

When `overview.md` exists, use:

```yaml
---
id: wiki-overview
type: wiki_overview
title: "Wiki overview"
created: 2026-09-01
updated: 2026-09-01
as_of: 2026-09-01
summary: "A selective map of the Wiki's confirmed durable picture."
evidence_refs:
  - wiki-concept-example
last_confirmed_commit: kc-20260901T120000Z-a1b2c3
---
```

Every material overview statement must cite a vault-root-qualified link to a
listed evidence page. `evidence_refs` may include any knowledge-page ID because
the overview summarizes the compiled Wiki; it does not replace those pages or
act as their evidence. The overview has no relationship edges and never embeds
live Project status as durable knowledge. Register it in the manifest as the
special `wiki_overview` artifact; its freshness dependencies are exactly these
`evidence_refs`, while semantic wording review remains confirmation-bound.
