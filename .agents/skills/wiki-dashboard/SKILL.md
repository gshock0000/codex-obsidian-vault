---
name: wiki-dashboard
description: Build or refresh navigational Wiki dashboards from existing records without turning live project state into durable knowledge. Use when the user asks for a knowledge overview, dashboard, or vault status view.
---

# Wiki dashboard

Dashboards are derived navigation and review views, not new evidence. Read the
[Wiki page contract](../vault-governance/references/wiki-page-contract.md) and
[Wiki state contract](../vault-governance/references/wiki-state-contract.md).

## Choose the view type

### Thin index

`06_wiki/index.md` is a deterministic catalog: accepted, provisional, and
disputed pages appear once under type; archived and superseded pages appear
once in `History`. Each uses a path-qualified link and current summary. The
index does not rank truth, infer importance, or summarize live work.

### Durable overview

`06_wiki/overview.md` explains the Wiki's confirmed high-level picture. Its
selection and wording can influence meaning, so create or update it only as an
explicit part of a confirmed knowledge-commit proposal. Do not refresh it after
every ingest when the global picture did not change.

### Operational dashboard

Freshness, lint findings, pending reviews, graph gaps, and live project status
are operational views. Return them in chat by default. If the user asks to save
one, use `01_inbox/report/` unless they specify another non-Wiki location. Do not
turn a status snapshot into a knowledge page.

## Data sources

Enumerate actual records; do not trust a previous dashboard as source data.
Depending on the requested view, read:

- Wiki page frontmatter and summaries;
- `06_wiki/log/` for confirmed commit history;
- `.state/manifest.json` for confirmed baselines and dependencies;
- `.state/observations.json` for scoped effective freshness, with its cutoff and
  only when its baseline-manifest hash matches;
- current lint findings or incomplete transactions;
- explicitly requested live Project records.

Check `.state/write.lock` and transactions first. A lock or
`preparing`/`prepared` operation is pending; an `applying`/`needs-recovery`
operation makes the visible Wiki potentially inconsistent. Put that state
first, use the verified all-preimage overlay for any last-success view, and do
not regenerate index/overview until recovery completes. For a valid
`observation_refresh` lock, ignore the cache being atomically replaced and
build a read-only view from the manifest plus in-memory observations.

When Project data is included, enumerate and reconcile the relevant Project
files under the project-management rules. Label it **live project context** and
keep it visually separate from **confirmed Wiki knowledge**.

## Build rules

- Link every item to its page or record and label the observation date.
- Distinguish semantic lifecycle, confirmed review state, and the effective
  review overlay from current/cached observations.
- Include `wiki-overview`'s special manifest review state when it exists; source
  drift may affect it without authorizing a wording refresh.
- Derive counts from the enumerated set; report the denominator and excluded
  scope. Never invent counts or completion.
- Useful sections include recent confirmed commits, resources needing review,
  downstream affected pages, missing evidence, contradictions awaiting review,
  synthesis gaps, and incomplete transactions.
- Graph hubs, bridges, or orphans may prioritize inspection. Do not create
  links, confidence scores, lifecycle changes, or syntheses from graph metrics
  alone.
- Do not copy full page bodies into a dashboard. Use titles, summaries, state,
  and focused explanations.
- If manifest or index data is missing or inconsistent, show the limitation
  instead of silently repairing it.
- Do not refresh remote sources unless the user explicitly requests a network
  freshness scope; otherwise display the cache cutoff or `unverifiable`.

## Write boundary

Dashboard generation is read-only by default and does not append a query or
review log. Before writing any persistent view, show its location, data cutoff,
sections, and proposed content.

- An index-only deterministic regeneration is `wiki_maintenance`; apply its
  confirmed exact diff through the knowledge-commit transaction protocol.
- An overview change follows the knowledge-commit protocol.
- A saved operational report is source-layer output and must not alter Wiki
  semantics.

Never combine a dashboard refresh with unreviewed semantic page repairs.
