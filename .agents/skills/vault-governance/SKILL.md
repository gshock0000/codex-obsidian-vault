---
name: vault-governance
description: Govern the six-layer personal knowledge vault when routing, changing, ingesting, or reviewing its content. Use for decisions that cross Inbox, Project, Area, Resource, Archive, and Wiki.
---

# Vault governance

Maintain one local, human-readable knowledge vault. The user owns and directly maintains `01_inbox`, `02_Project`, `03_Area`, and `04_Resource`; `06_wiki` contains only knowledge the user has confirmed for promotion.

## Canonical layout and ownership

| Path | Role | Ingest rule |
| --- | --- | --- |
| `01_inbox/` | New ideas, reflections, reading notes, research reports, and untriaged input | Opt-in: ingest only after the user explicitly selects material. |
| `01_inbox/raw/` | Original captures, downloaded documents, and research source material | Immutable evidence; opt-in with its selected parent material. |
| `02_Project/<project>/` | Current project records and project-local knowledge | Opt-in: ingest only after the user explicitly selects material. |
| `03_Area/<area>/` | User-maintained, long-lived domain archive and active notes | Continuous sync source: detect changes and prepare Wiki proposals. |
| `04_Resource/` | User-curated important resources | Continuous sync source: detect changes and prepare Wiki proposals. |
| `05_Archived/` | Closed or infrequently used material | Never include in new ingest work. Keep existing provenance valid. |
| `06_wiki/` | Confirmed, agent-maintained compiled knowledge | Do not write until the user confirms a proposal. |

Do not duplicate an external original into a project. Keep the only original copy in `01_inbox/raw/`; project `knowledge/` pages record project-specific use, context, and links to the original.

## Core invariants

- Preserve `01_inbox/raw/` byte-for-byte. Do not normalize, rename, or annotate an original file; use a sidecar or a separate Markdown page for metadata.
- Keep everyday source-note YAML simple. Every editable source note uses `title`, `type`, `status`, `created`, `updated`, `tags`, `source`, and `summary`; do not require manually maintained IDs. The ingest manifest assigns a stable source ID when a note first participates in Wiki provenance. Paths remain locators and may change after archival.
- `status` is a required page-lifecycle field. Choose concise values that fit the type, such as plan `ongoing`/`done`, issue `ongoing`/`solved`, idea `ongoing`/`done`/`invalid`, or resource `saved`/`reviewed`/`stale`/`archived`.
- Treat `02_Project`, `03_Area`, and `04_Resource` as source material, not as a duplicate of the Wiki.
- Treat the Wiki as a compiled graph, not a mirror. A changed source makes a derived page `needs-review`; it does not authorize a silent rewrite.
- Treat `01_inbox` and `02_Project` as opt-in sources: do not discover or propose new Wiki promotions without an explicit user ingest request. Treat `03_Area` and `04_Resource` as continuous sync sources: automatically detect changes, reconcile their existing Wiki dependencies, and prepare any required proposals.
- A previously ingested Inbox or Project source remains eligible for lint freshness checks, but this never authorizes a new promotion.
- Use modification time only to find candidate changes. Confirm local changes with a content hash; use a remote revision or ETag when it exists.
- Keep source-to-Wiki dependencies and last-observed fingerprints in a machine-managed manifest. It is rebuildable state, not a user-authored second source of truth.
- Preserve disagreement and superseded material. Never remove old evidence merely to make a current conclusion look simpler.

## Routing

- Use `project-management` for project records, project-local knowledge cards, and weekly reports.
- Use `area-management` for long-lived domain notes and area structure.
- Use `resource-management` for curated resources in `04_Resource`.
- Use `research-capture` when performing external research.
- Use the focused `wiki-*` skill for an operation on `06_wiki`.

When a request could fit more than one layer, first decide whether it is current work context, durable source material, or confirmed reusable knowledge. Ask the user only when that distinction affects where a new record is written.
