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
├─ plans/                          # plans and milestones
├─ decisions/                      # one decision per file
├─ activities/                     # one AP, issue, test, or project idea per file
├─ knowledge/                      # project-local knowledge cards, without fixed subfolders
└─ wk_reports/                     # one derived weekly report per ISO week

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
- `01_inbox/raw` is immutable. Do not alter, rename, normalize, annotate, or overwrite an original source. Add separate metadata only when it is needed for research or Wiki provenance.
- An external original exists only once: retain datasheets, vendor guides, downloaded documents, page captures, and research source material under `01_inbox/raw`.
- A project `knowledge/` card records project-specific usage and links to the original source by its local path or URL; it must not duplicate the original file. The ingest manifest adds a stable source ID only if Wiki provenance needs one.
- `05_Archived` is excluded from all new ingest operations. Existing Wiki provenance may still resolve to archived sources.
- Do not require manually maintained IDs in normal source notes. When a source first participates in a Wiki ingest, the ingest manifest assigns and preserves a stable source ID; file paths remain mutable locators.
- Preserve superseded decisions, failed tests, and conflicting evidence. Mark their current lifecycle instead of deleting history.

## Wiki source policy

- `01_inbox` and `02_Project` are opt-in Wiki sources. Do not discover, propose, or promote new Wiki knowledge from them unless the user explicitly asks to ingest the specified material. This includes ideas, research reports, project decisions, activities, and project knowledge cards.
- `03_Area` and `04_Resource` are continuous Wiki sources. Their changes must be included in normal sync and lint work without requiring a separate ingest request from the user.
- Continuous sync does not make the Wiki a mirror: compile only durable, relevant understanding and retain source provenance. A changed Area or Resource item must create a review/proposal for affected Wiki content rather than silently overwrite meaning.
- If an Inbox or Project record has already been promoted, its existing source dependency remains eligible for lint freshness checks; this does not authorize a new promotion.
- All semantic Wiki writes still require a user-confirmed knowledge-commit proposal. Continuous sync autonomously detects drift and prepares that proposal.

## Project records

Use one Markdown file per record with an independent meaning or lifecycle. Keep Markdown body readable and use YAML frontmatter for stable fields.

All editable source notes use: `title`, `type`, `status`, `created`, `updated`, `tags`, `source`, and `summary`. Project records additionally use `project`; Area and Resource records additionally use `area`. Keep YAML short enough that a person will maintain it. Use project types such as `plan`, `decision`, `activity`, `issue`, `test`, `idea`, `knowledge`, and `wk_report` when they add clarity.

`status` is required and records the page lifecycle. Choose short, lowercase values that fit the type rather than forcing one universal vocabulary: for example, plans use `ongoing` or `done`; issues use `ongoing` or `solved`; ideas use `ongoing`, `done`, or `invalid`; resources commonly use `saved`, `reviewed`, `used`, `stale`, or `archived`.

- Decisions record the choice, evidence or confirmation, alternatives, and supersession where applicable. Do not create a separate confirmation record merely because a decision cites a confirmation.
- Activities use one file per AP (action point), issue, test, or project idea. An activity records the work, its current `status`, its evidence, and the next action where useful. Keep resolved activities as history rather than deleting them.
- Knowledge cards record their source references and project-specific usage notes. Keep all cards directly under `knowledge/` until a future need justifies further organization.

`overview.md` is the live project view. It shows current overall status, current focus, AP counts, unresolved issue count, blockers or risks, upcoming plan items, and links to the latest weekly report. It is derived from the project records, not a replacement for them.

Weekly reports live in `wk_reports/` and are derived status snapshots. Before creating or updating one, inspect every `activities/**/*.md` record, the immediately preceding weekly report, current `plans/**/*.md`, relevant `decisions/**/*.md`, and `overview.md`. Identify status changes rather than merely restating records; reconcile plan progress with the previous week's commitments; do not invent counts or completions. Link material statements to their supporting records. Weekly reports are not default Wiki sources.

## Area notes

`03_Area` contains evolving domain notes as well as durable references. Use hierarchical area identifiers such as `rf/antenna-design`; use tags for subjects rather than ownership.

When an Area note is eligible for later Wiki promotion, `wiki-ingest` may add a synchronization mode:

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

1. source paths, relevant headings, confirmations, and data references; include a manifest source ID only when one already exists;
2. proposed `resources/` pages and their evidence;
3. proposed changes to concepts, entities, claims, or syntheses;
4. whether each change is add, refine, supersede, or contradiction;
5. confidence and unresolved uncertainty.

Source-specific default rules:

- From `01_inbox`, ingest only material the user explicitly selects. Promote stable learning or evidence-backed findings only; do not promote raw ideas or unverified conclusions.
- From `02_Project`, ingest only material the user explicitly selects. If selected, consider verified decisions, activities with reusable evidence or lessons, and project knowledge cards; exclude plans, `overview.md`, weekly reports, TODOs, and unverified issue hypotheses by default.
- From `03_Area`, continuously synchronize stable and reusable learning without copying the entire note.
- From `04_Resource`, continuously synchronize reviewed or relevant resources without waiting for an explicit ingest request.
- Never perform a new ingest from `05_Archived`.

Create an evidence-backed `06_wiki/resources/` page before deriving a material concept or claim. Use resource pages to preserve the chain from project decision, person/team confirmation, and test data to a conclusion.

The detailed frontmatter for Wiki page types is intentionally undecided until a reference LLM-Wiki implementation is selected. Ingest and lint tracking belongs in the machine-managed manifest and the eventual Wiki page schema, not in everyday source-note templates.

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

Answer durable-knowledge questions from `06_wiki`, citing internal pages and disclosing stale or missing evidence. For a question about a project, first enumerate and search the project's Markdown files, then read the relevant records before reasoning or answering. Read `overview.md` as the live summary but verify it against `activities/`, `plans/`, `decisions/`, `knowledge/`, and `wk_reports/` as the question requires. Label the result as live project context. A useful answer is not automatically a Wiki update; offer a synthesis proposal when it has standalone long-term value.
