# AI Vault agent instructions

## Purpose

This repository is a local-first personal knowledge vault. It holds personal learning, domain knowledge, project work, curated resources, and a user-confirmed LLM Wiki. The vault must remain human-readable, traceable, and usable without a database or a specific application.

The user is the primary curator of active material. The agent organizes, summarizes, proposes, and maintains; it does not silently turn working notes into durable knowledge.

## Start here

For any request in this repository:

1. Read this file.
2. Identify the relevant vault layer before reading or writing files.
3. Load the focused project skill from `.agents/skills/` before carrying out its specialized workflow.
4. Preserve the distinction between current work context and confirmed reusable knowledge.

Project-local Skills live only in `.agents/skills/`. Read and update the relevant `SKILL.md` there.

## Vault structure

```text
01_inbox/                         # new and untriaged input
├─ ideas/                          # new ideas and early thoughts
├─ reflections/                    # reading reflections and personal notes
├─ reading/                        # active reading material and notes
├─ research/                       # agent-generated external research reports
├─ report/                         # other generated reports awaiting use
└─ raw/                            # immutable original captures and documents

02_Project/<project>/             # one folder per active project
├─ overview.md                     # navigation and current context
├─ status/                         # dated weekly/project status snapshots
├─ plans/                          # plans and milestones
├─ decisions/                      # one decision per file
├─ confirmations/                  # one confirmation per file
├─ tests/                          # one test/result record per file
├─ issues/                         # one issue or hypothesis per file
├─ ideas/                          # project-specific ideas
└─ knowledge/                      # project-local knowledge cards
   ├─ components/
   ├─ guides/
   ├─ architecture/
   ├─ glossary/
   └─ references/

03_Area/<area>/                   # long-lived user-maintained domain archives
04_Resource/                      # user-curated important resources
05_Archived/                      # closed/infrequently used material
└─ projects/                       # archived project folders when requested

06_wiki/                          # user-confirmed compiled knowledge
├─ index.md                        # thin navigation catalog
├─ overview.md                     # whole-Wiki overview
├─ log/                            # append-only knowledge commit and review logs
├─ resources/                      # evidence-backed resource pages
├─ concepts/                       # reusable concepts, methods, and patterns
├─ entities/                       # people, teams, projects, components, tools
├─ syntheses/                      # durable reports, comparisons, and briefs
└─ claims/                         # high-value, evidence-backed assertions only
```

Create only directories needed by confirmed work. Do not create placeholder project, area, or Wiki pages merely to complete this tree.

## Ownership and source rules

- `01_inbox`, `02_Project`, `03_Area`, and `04_Resource` are user-maintained source layers. They are not mirrors of `06_wiki`.
- `01_inbox/raw` is immutable. Do not alter, rename, normalize, annotate, or overwrite an original source. Store metadata in a sidecar or a separate Markdown record.
- An external original exists only once: retain datasheets, vendor guides, downloaded documents, page captures, and research source material under `01_inbox/raw`.
- A project `knowledge/` card records project-specific usage and links to the original source by stable ID; it must not duplicate the original file.
- `05_Archived` is excluded from all new ingest operations. Existing Wiki provenance may still resolve to archived sources.
- Every durable source record needs a stable `id`. File paths are locators and may change when an item is archived.
- Preserve superseded decisions, failed tests, and conflicting evidence. Mark their current lifecycle instead of deleting history.

## Project records

Use one Markdown file per record with an independent meaning or lifecycle. Keep Markdown body readable and use YAML frontmatter for stable fields.

All atomic project records use: `id`, `type`, `title`, `project`, `status`, `created`, `updated`, and `tags`.

- Decisions also record decision date, alternatives, and supersession where applicable.
- Confirmations record who confirmed what, when, scope, and supporting evidence.
- Tests record method, result, data references, date, and verification state.
- Issues distinguish observation from hypothesis and record the next validation step.
- Knowledge cards record their source references and project-specific usage notes.

Weekly reports are derived status snapshots. Link each statement to the underlying records and include completed work, new decisions/confirmations, test results, open risks, plan changes, and next-week focus. Weekly reports are not default Wiki sources.

## Area notes

`03_Area` contains evolving domain notes as well as durable references. Use hierarchical area identifiers such as `rf/antenna-design`; use tags for subjects rather than ownership.

When an Area note is eligible for later Wiki promotion, choose one synchronization mode:

- `append` for normally growing logs and learning notes. After a confirmed promotion, record a `<!-- wiki-commit: <id> -->` marker; later ingest examines the tail first.
- `snapshot` for notes that may be revised anywhere. Detect changes by content fingerprint and mark dependent Wiki pages for review.

An append marker is not proof that earlier content is unchanged. An edit before the marker requires review of affected Wiki resources.

## External research

When performing external research intended for the vault:

1. Preserve original source material or capture metadata under `01_inbox/raw/` without editing it.
2. Save the agent's synthesized report, with sources and date, under `01_inbox/research/` or `01_inbox/report/`.
3. Keep source fact separate from inference.
4. Do not write research findings directly to `06_wiki`.

## Wiki promotion and structure

`06_wiki` is compiled knowledge, not a copy of its sources. All Wiki writes require a user-confirmed knowledge-commit proposal.

Before proposing a Wiki change, state:

1. source IDs, paths, relevant headings, confirmations, and data references;
2. proposed `resources/` pages and their evidence;
3. proposed changes to concepts, entities, claims, or syntheses;
4. whether each change is add, refine, supersede, or contradiction;
5. confidence and unresolved uncertainty.

Source-specific default rules:

- From `01_inbox`, promote stable learning or evidence-backed findings only; do not promote raw ideas or unverified conclusions.
- From `02_Project`, consider verified decisions, confirmations, tests, reusable debug lessons, and explicitly selected knowledge cards. Exclude plans, progress, weekly status, TODOs, and unverified issue hypotheses by default.
- From `03_Area`, compile stable and reusable learning without copying the entire note.
- From `04_Resource`, use only reviewed resources or items selected by the user.
- Never perform a new ingest from `05_Archived`.

Create an evidence-backed `06_wiki/resources/` page before deriving a material concept or claim. Use resource pages to preserve the chain from project decision, person/team confirmation, and test data to a conclusion.

Every Wiki page uses a compact YAML frontmatter with at least `id`, `type`, `title`, `status`, `created`, `updated`, and `tags`. Resource pages also use `origins`, `source_refs`, `relations`, and `sync`.

Use only these initial relationship types: `derived_from`, `supports`, `refines`, `contradicts`, `applies_to`, `uses`, and `supersedes`.

## Freshness and linting

Treat freshness as a provenance question, not text equality between a source and the Wiki.

- Use modification time only to find local files that may have changed.
- Compare content hashes to confirm local changes; use revision or ETag for remote sources when available.
- Keep last-observed source fingerprints and dependency status in rebuildable machine-managed manifest state.
- When a source changes, flag its direct Wiki resource as `needs-review`; flag downstream concepts, claims, and syntheses as affected. Do not silently rewrite them.
- Lint checks links, IDs, frontmatter, relationship vocabulary, source existence, source drift, archived paths, append-marker changes, orphans, contradictions, stale pages, and index drift.
- Structural repairs and semantic changes must be presented separately. Never perform semantic repairs without user confirmation.

## Query behavior

Answer durable-knowledge questions from `06_wiki`, citing internal pages and disclosing stale or missing evidence. For current project progress, plans, open issues, or status, use `02_Project` as the source of truth and label the result as live project context. A useful answer is not automatically a Wiki update; offer a synthesis proposal when it has standalone long-term value.
