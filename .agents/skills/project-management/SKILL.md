---
name: project-management
description: Manage atomic project records, project-local knowledge cards, and weekly project status reports under 02_Project. Use for project decisions, confirmations, tests, issues, plans, and project knowledge.
---

# Project management

Project folders are the source of truth for work in progress. Keep a distinct file for every record with an independent meaning or lifecycle; do not hide decisions, confirmations, or test results inside a general project journal.

## Project shape

Use only the subfolders the project needs:

```text
02_Project/<project>/
├─ overview.md
├─ status/
├─ plans/
├─ decisions/
├─ confirmations/
├─ tests/
├─ issues/
├─ ideas/
└─ knowledge/
   ├─ components/
   ├─ guides/
   ├─ architecture/
   ├─ glossary/
   └─ references/
```

`overview.md` is navigation and current context. It is not a default Wiki source. `knowledge/` records how this project uses an item; originals such as datasheets and vendor guides remain solely under `01_inbox/raw/` and are referenced by stable source ID.

## Record metadata

Use Markdown body plus compact YAML frontmatter. Every atomic record should have `id`, `type`, `title`, `project`, `status`, `created`, `updated`, and `tags`. Add only fields that change the record's meaning.

- Decisions: `decision_date`, `supersedes`, `alternatives`.
- Confirmations: `confirmed_by`, `confirmed_at`, `confirmation_scope`, and an evidence reference.
- Tests: `test_date`, `method`, `result`, `data_refs`, and `verification_status`.
- Issues: `severity`, `hypothesis_status`, `next_check`, and `resolved_by` when closed.
- Knowledge cards: `source_refs`, `component_or_topic`, and project-specific `usage_notes`.

Use lifecycle states appropriate to the record, such as `active`, `verified`, `resolved`, `superseded`, `closed`, or `archived`. Do not delete a superseded decision or failed test.

## Weekly status reports

When asked for a weekly report, create or update one status snapshot for that ISO week. Summarize only evidence in the project's records and label uncertainty.

Include: completed work, decisions/confirmations obtained, tests and results, open issues or risks, plan changes, and next-week focus. Link to the underlying atomic records. A weekly report is a derived working view and is not ingested into the Wiki by default.

## Wiki boundary

Plans, progress, live status, TODOs, and unverified issue hypotheses do not enter `06_wiki` by default. A verified decision, confirmation, test lesson, or reusable debugging result may be proposed to `wiki/resources` with complete evidence. Use `wiki-ingest` for that proposal; never promote it silently.

Only move a finished project to `05_Archived/projects/` when the user requests it. Preserve record IDs so existing Wiki provenance can be reconciled after the path changes.
