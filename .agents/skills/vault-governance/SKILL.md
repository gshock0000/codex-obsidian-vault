---
name: vault-governance
description: Govern the six-layer personal knowledge vault when routing, changing, ingesting, or reviewing its content. Use for decisions that cross Inbox, Project, Area, Resource, Archive, and Wiki.
---

# Vault governance

Maintain one local, human-readable knowledge vault. The user owns and directly maintains `01_inbox`, `02_Project`, `03_Area`, and `04_Resource`; `06_wiki` contains only knowledge the user has confirmed for promotion.

## Canonical layout and ownership

| Path | Role | Ingest rule |
| --- | --- | --- |
| `01_inbox/` | New ideas, reflections, reading notes, research reports, and untriaged input | Route by input type. |
| `01_inbox/raw/` | Original captures, downloaded documents, and research source material | Immutable; use only as evidence. |
| `02_Project/<project>/` | Current project records and project-local knowledge | Read selectively; plans and live status do not become Wiki knowledge by default. |
| `03_Area/<area>/` | User-maintained, long-lived domain archive and active notes | May provide candidate knowledge. |
| `04_Resource/` | User-curated important resources | Create source/resource candidates only when the material has been read and is relevant. |
| `05_Archived/` | Closed or infrequently used material | Never include in new ingest work. Keep existing provenance valid. |
| `06_wiki/` | Confirmed, agent-maintained compiled knowledge | Do not write until the user confirms a proposal. |

Do not duplicate an external original into a project. Keep the only original copy in `01_inbox/raw/`; project `knowledge/` pages record project-specific use, context, and links to the original.

## Core invariants

- Preserve `01_inbox/raw/` byte-for-byte. Do not normalize, rename, or annotate an original file; use a sidecar or a separate Markdown page for metadata.
- Give source records stable IDs. Paths are locators, not identities: a record may move to `05_Archived` without breaking Wiki provenance.
- Treat `02_Project`, `03_Area`, and `04_Resource` as source material, not as a duplicate of the Wiki.
- Treat the Wiki as a compiled graph, not a mirror. A changed source makes a derived page `needs-review`; it does not authorize a silent rewrite.
- Use modification time only to find candidate changes. Confirm local changes with a content hash; use a remote revision or ETag when it exists.
- Record source-to-Wiki dependencies in page frontmatter and keep last-observed fingerprints in a machine-managed manifest. The manifest is rebuildable state, not a user-authored second source of truth.
- Preserve disagreement and superseded material. Never remove old evidence merely to make a current conclusion look simpler.

## Routing

- Use `project-management` for project records, project-local knowledge cards, and weekly reports.
- Use `area-management` for long-lived domain notes and area structure.
- Use `resource-management` for curated resources in `04_Resource`.
- Use `research-capture` when performing external research.
- Use the focused `wiki-*` skill for an operation on `06_wiki`.

When a request could fit more than one layer, first decide whether it is current work context, durable source material, or confirmed reusable knowledge. Ask the user only when that distinction affects where a new record is written.
