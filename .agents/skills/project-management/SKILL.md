---
name: project-management
description: Manage project plans, decisions, activities, local knowledge, live overviews, and weekly reports under 02_Project.
---

# Project management

A project folder is the source of truth for active work. Keep each record in its own Markdown file when it has an independent meaning or lifecycle. `overview.md` is a derived, live view; it must never replace the underlying records.

## Project shape

```text
02_Project/<project>/
├─ overview.md
├─ plans/
├─ decisions/
├─ activities/
├─ knowledge/
└─ wk_reports/
```

- `plans/`: plans and milestones.
- `decisions/`: decisions, including the evidence or external confirmation that supports them. Do not create a separate confirmation folder.
- `activities/`: one AP (action point), issue, test, or project idea per file. The record `type` distinguishes these cases; do not create separate issue, test, or idea folders.
- `knowledge/`: project-local knowledge cards, directly in this folder until further grouping is genuinely needed. Original external material remains only in `01_inbox/raw/` and is linked by local path or URL.
- `wk_reports/`: one derived weekly report per ISO week.

## Record metadata and lifecycle

Every editable project note uses `title`, `type`, `status`, `project`, `created`, `updated`, `tags`, `source`, and `summary`. Do not add manually maintained IDs or Wiki tracking fields.

Use a concise `status` that fits the record:

- plans: `ongoing` or `done`;
- activities and issues: `ongoing` or `solved`;
- ideas: `ongoing`, `done`, or `invalid`;
- tests: `ongoing`, `done`, `verified`, or `failed`;
- decisions: `ongoing`, `done`, or `superseded`.

Add fields only when they clarify the record. Decisions may include alternatives and supersession. Activities may include an owner, next action, test result, evidence, or a link to the related plan. Keep resolved activities and superseded decisions as history.

## Live overview

When creating or rebuilding this page, use [the project-generated page templates](references/project-page-templates.md). Those templates are agent-internal and must not be copied into Obsidian's `Templates/` folder.
`overview.md` must show the present project state, not a narrative copy of old reports. Maintain:

- overall health (`on_track`, `at_risk`, or `blocked`), current phase, current focus, and the last meaningful change;
- counts of ongoing APs, APs completed this week, unresolved issues, and blocked APs;
- current blockers or risks, upcoming plan items, and navigation to the latest weekly report, relevant plans, key decisions, and active activities.

Derive counts and status from project records. State the check date and do not invent a count if records are incomplete. When an agent changes a plan, decision, or activity that materially changes current project state, update `overview.md` in the same task unless the user asks not to.

## Weekly reports

Use [the project-generated page templates](references/project-page-templates.md) for this report's page structure.
Create one report in `wk_reports/` for each ISO week, named `YYYY-Www.md`. A weekly report is a derived snapshot and is not a default Wiki source.

Before writing a weekly report:

1. Inspect every Markdown file under `activities/`, including resolved records, so status changes and carried work are not missed.
2. Read the immediately preceding weekly report and identify commitments, unresolved items, and the prior overall status.
3. Read current Markdown files under `plans/` and relevant files under `decisions/`; compare actual activity progress with the plan.
4. Read `overview.md`, then reconcile it with the underlying records rather than trusting it blindly.
5. Report only evidence from those files. Link material statements to their activity, plan, or decision records; label missing evidence or uncertainty.

The report must cover: overall status and its change from last week, completed APs, ongoing/blocked APs and unresolved issues, plan progress or change, decisions or decisions needed, risks or asks, and next-week focus. After creation, update `overview.md` if the report establishes a change in the current state.

## Project questions

When the user asks about a project, first enumerate and search all Markdown files in that project folder. Then read the files relevant to the question before reasoning or answering; never answer solely from `overview.md` or the most recent weekly report.

For a status question, read `overview.md`, all relevant activities, current plans, the latest weekly report, and relevant decisions. For a historical or technical question, search the entire project first and cite the records actually used. If the required records do not exist, say so rather than inferring an answer.

## Wiki boundary

`02_Project` is opt-in for Wiki ingest. Do not propose or promote new Wiki knowledge unless the user explicitly selects the project material. If selected, verified decisions, activities with reusable evidence or lessons, and knowledge cards may be candidates. Plans, `overview.md`, weekly reports, TODOs, and unverified hypotheses are excluded by default.

Only move a finished project to `05_Archived/projects/` when the user requests it. Existing Wiki provenance remains reconciled through manifest-issued source IDs after paths change.