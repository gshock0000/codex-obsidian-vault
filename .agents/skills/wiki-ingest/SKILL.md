---
name: wiki-ingest
description: Propose evidence-backed promotion from Inbox, Project, Area, or Resource into 06_wiki. Use when the user asks to ingest, learn from, or commit material to the LLM Wiki.
---

# Wiki ingest

Compile confirmed knowledge into `06_wiki`; do not mirror source folders or silently edit existing Wiki pages.

## Source-specific intake

- `01_inbox/raw`: use as immutable evidence. Create a faithful resource summary only when it is relevant to the requested promotion.
- `01_inbox/ideas`, reading notes, and research reports: extract only stable questions, observations, or evidence-backed learning; keep speculative ideas out of the Wiki.
- `02_Project`: consider verified decisions, confirmations, test results, reusable debugging lessons, and explicitly designated project knowledge cards. Skip plans, progress, weekly status, TODOs, and unverified issue hypotheses unless the user explicitly asks otherwise.
- `03_Area`: use the declared `append` or `snapshot` mode. Compile stable learning without duplicating the entire Area note.
- `04_Resource`: ingest only material that has been reviewed or is explicitly selected by the user.
- `05_Archived`: never use for a new ingest.

## Proposal before write

Before changing `06_wiki`, present a concise knowledge-commit proposal containing:

1. source IDs, paths, and relevant headings or data references;
2. proposed `wiki/resources` pages and their evidence;
3. concept, entity, claim, or synthesis pages that would be created or changed;
4. whether each change is add, refine, supersede, or contradiction;
5. confidence and unresolved uncertainty.

Write only after the user confirms the proposal.

## Resource pages

Create `wiki/resources` before promoting a conclusion into a concept or claim. A resource page may combine closely related project evidence, such as a decision, a confirmation, and a test, but must preserve each source reference separately.

Use `id`, `type`, `title`, `status`, `created`, `updated`, `tags`, `origins`, `source_refs`, `relations`, and `sync` in resource frontmatter. For project evidence, include evidence kind and relevant fields such as `confirmed_by`, `confirmed_at`, `data_refs`, or test method.

Use only these typed relationships initially: `derived_from`, `supports`, `refines`, `contradicts`, `applies_to`, `uses`, and `supersedes`.
