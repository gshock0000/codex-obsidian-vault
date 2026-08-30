---
name: wiki-ingest
description: Propose evidence-backed promotion from Inbox, Project, Area, or Resource into 06_wiki. Use when the user asks to ingest, learn from, or commit material to the LLM Wiki.
---

# Wiki ingest

Compile confirmed knowledge into `06_wiki`; do not mirror source folders or silently edit existing Wiki pages.

## Source-specific intake

- `01_inbox/raw`: use as immutable evidence only when the user explicitly requests ingestion of the relevant material.
- `01_inbox/ideas`, reading notes, and research reports: do not discover or promote them unless the user explicitly requests ingest. When selected, extract only stable questions, observations, or evidence-backed learning; keep speculative ideas out of the Wiki.
- `02_Project`: do not discover or promote project records unless the user explicitly requests ingest. When selected, consider verified decisions, activities with reusable evidence or lessons, and project knowledge cards. Skip plans, `overview.md`, weekly reports, TODOs, and unverified issue hypotheses unless the user explicitly asks otherwise.
- `03_Area`: continuously scan and reconcile Area material through the declared `append` or `snapshot` mode. Compile stable learning without duplicating the entire Area note.
- `04_Resource`: continuously scan and reconcile reviewed or relevant resources; no explicit ingest request is required.
- `05_Archived`: never use for a new ingest.

## Proposal before write

Before changing `06_wiki`, present a concise knowledge-commit proposal containing:

1. source paths and relevant headings or data references; assign stable source IDs in the ingest manifest when this is the first promotion;
2. proposed `wiki/resources` pages and their evidence;
3. concept, entity, claim, or synthesis pages that would be created or changed;
4. whether each change is add, refine, supersede, or contradiction;
5. confidence and unresolved uncertainty.

Write only after the user confirms the proposal.

## Resource pages

Create `wiki/resources` before promoting a conclusion into a concept or claim. A resource page may combine closely related project evidence, such as a decision, a confirmation, and a test, but must preserve each source reference separately.

The final frontmatter schema for Wiki resource pages remains intentionally undecided until a reference LLM-Wiki implementation is selected. Keep provenance, source IDs, fingerprints, and review status in the ingest manifest until that schema is agreed. For project evidence, preserve evidence kind and relevant fields such as `confirmed_by`, `confirmed_at`, `data_refs`, or test method.

Use only these typed relationships initially: `derived_from`, `supports`, `refines`, `contradicts`, `applies_to`, `uses`, and `supersedes`.
