---
name: vault-governance
description: Govern the six-layer personal knowledge vault when routing, changing, ingesting, or reviewing its content. Use for decisions that cross Inbox, Project, Area, Resource, Archive, and Wiki.
---

# Vault governance

Maintain one local, human-readable knowledge vault. The user owns and directly maintains `01_inbox`, `02_Project`, `03_Area`, and `04_Resource`; `06_wiki` contains only knowledge the user has confirmed for promotion.

## Wiki contract

Before any Wiki ingest, lint, query, synthesis, or dashboard operation, read the
focused `wiki-*` skill. Load these shared references when the operation touches
their subject:

- [Wiki page contract](references/wiki-page-contract.md) for page schema, body
  requirements, evidence fields, relationships, index, and overview;
- [Wiki state contract](references/wiki-state-contract.md) for source identity,
  hashes, append/snapshot behavior, manifest state, and dependency impact;
- [Knowledge-commit protocol](references/knowledge-commit-protocol.md) for
  proposals, confirmation, conflict checks, multi-file writes, recovery, and
  logs.
- [Wiki operation acceptance scenarios](references/wiki-operation-scenarios.md)
  when changing, testing, or resolving ambiguity in these workflows.

These references are canonical. Do not copy a conflicting schema into a focused
skill or infer a field from an external reference project.

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
- Keep confirmed source-to-Wiki dependencies and committed baselines in the
  hash-chained, rebuildable `06_wiki/.state/manifest.json`. Keep scoped current
  observations in disposable `observations.json`; keep active staged payloads
  in non-rebuildable transaction journals.
- Keep a Wiki page's confirmed semantic `status` separate from computed
  `review_state`. Drift requests review; it never adjudicates truth.
- Create an evidence-backed Wiki resource before a material concept, entity,
  claim, or synthesis. Preserve exact source locators and distinguish source
  fact from inference.
- Use one orchestrator for a Wiki write set. Subagents may research, extract,
  challenge, and draft, but cannot independently commit semantic pages.
- Serialize every persistent Wiki writer through `.state/write.lock`; correlate
  an existing lock with its recovery journal and never infer staleness from age
  alone.
- Preserve disagreement and superseded material. Never remove old evidence merely to make a current conclusion look simpler.

## Routing decision

Before writing, classify the material:

1. Is it untriaged input or an external research capture? Route it to
   `01_inbox`; use `research-capture` for external research.
2. Is it current work toward a bounded outcome? Route it to `02_Project`.
3. Is it an evolving responsibility or domain archive? Route it to `03_Area`.
4. Is it a deliberately curated source worth returning to? Route it to
   `04_Resource`.
5. Is it inactive history? Keep or move it under `05_Archived` only when the
   user requests that lifecycle action.
6. Is it evidence-backed, cross-context understanding that passed the
   compilation-value gate? Use `wiki-ingest` to prepare a proposal; do not write
   it directly.

When one input has multiple roles, keep one original and create lightweight
records that link to it. Do not solve ambiguity by copying content across
layers.

## Routing

- Use `project-management` for project records, project-local knowledge cards, and weekly reports.
- Use `area-management` for long-lived domain notes and area structure.
- Use `resource-management` for curated resources in `04_Resource`.
- Use `research-capture` when performing external research.
- Use the focused `wiki-*` skill for an operation on `06_wiki`.

When a request could fit more than one layer, first decide whether it is current work context, durable source material, or confirmed reusable knowledge. Ask the user only when that distinction affects where a new record is written.

## Mutation boundary

- Read-only diagnosis, query, and proposal drafting do not authorize writes.
- A request to create or update a source-layer record authorizes only that
  scoped source-layer work; it does not authorize Wiki promotion.
- Every durable Wiki change—semantic, re-verification, navigation, or structural
  repair—follows the knowledge-commit protocol, even during continuous sync.
- The only coupled source-layer mutation that protocol may include is the exact
  confirmed append marker on an eligible Area note. Any other source edit needs
  its own scoped authorization.
- An explicitly requested machine-state refresh may atomically replace only
  disposable `observations.json`, under the same exclusive write lock. It never
  changes the manifest or advances a confirmed source baseline.
- Lint repairs, navigation regeneration, and semantic changes are separate
  change sets. Present their effects separately and preserve unrelated edits.
