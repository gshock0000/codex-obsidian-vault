---
name: wiki-query
description: Answer questions from the compiled Wiki with clear provenance and distinguish durable knowledge from live project context. Use for questions about what the knowledge vault says.
---

# Wiki query

Answer from compiled knowledge without mutating it. Read the
[Wiki page contract](../vault-governance/references/wiki-page-contract.md) and
[Wiki state contract](../vault-governance/references/wiki-state-contract.md).

## Recovery gate

Inspect `.state/write.lock` and `.state/transactions/` before retrieval. If a
knowledge/maintenance lock or journal is `preparing` or `prepared`, use only the
verified last successful snapshot and disclose the pending operation. If it is
`applying` or `needs-recovery`, reconstruct the all-preimage last-success view
from verified preimages, successful logs, and the manifest; exclude new
desired-only pages. Never mix the physical partial tree with either transaction
overlay. A non-observation lock without a verifiable matching journal is itself
a consistency limitation. During a valid `observation_refresh` lock, ignore the
cache being replaced and compute any needed observation in memory; semantic
Wiki files remain readable.

If the journal/preimages are missing, corrupt, or conflict with actual hashes,
the Wiki is not a provably consistent snapshot. Stop any durable answer that
depends on affected targets, report the recovery requirement, and do not offer
a new synthesis proposal.

## Route the question first

- Use `06_wiki` for durable “what do we know?”, comparison, explanation, and
  cross-context questions.
- For a live project question, enumerate and search all Markdown under the
  relevant `02_Project/<project>/`. Read the relevant activities, plans,
  decisions, knowledge cards, weekly reports, and `overview.md`; reconcile the
  overview instead of trusting it alone. Label the result **live project
  context**.
- If the user explicitly asks for both, answer in two labeled sections. Do not
  make live project state look like confirmed Wiki knowledge.

## Progressive retrieval

Use the smallest evidence set that can answer reliably:

1. Read `06_wiki/index.md` when it exists. Search page IDs, titles, aliases,
   tags, and summaries across frontmatter.
2. Search relevant headings and body terms with `rg`; do not open every page by
   default.
3. Read the most relevant full pages, including their `evidence_refs`,
   `source_refs`, and relationship targets.
4. Traverse one evidence/relationship hop by default. Use a bounded,
   cycle-safe breadth-first traversal only when the question asks about a path,
   dependency, contradiction, or broader graph neighborhood.
5. Consult `.state/manifest.json` for the confirmed baseline and provenance.
   Read `.state/observations.json` when present, disclose its scan scope/cutoff,
   require its baseline-manifest hash to match, and compute current
   declared-dependency observations in memory when the answer requires fresher
   local evidence. Consult the matching commit log when a material assertion
   needs source-level support.

Do not use a fixed recent-file window or silently fall back to recollection. If
the index or manifest is missing/corrupt, search the page inventory directly and
state that navigation or freshness verification is incomplete.

Do not refresh a remote source unless the user explicitly includes network
freshness in the query scope. Otherwise use the cached observation and cutoff;
if no reliable revision/current bytes exist, label freshness `unverifiable`.

## Evidence and answer discipline

- Lead with the direct answer, then give the supporting internal pages and
  important limits.
- Cite path-qualified Wiki pages and relevant headings. When useful, cite the
  resource's source ID/locator, but do not bypass the resource page as the
  evidence bridge.
- Distinguish accepted, provisional, disputed, superseded, and archived meaning.
  Separately disclose confirmed versus effective `needs-review`,
  `missing-source`, `archived-source`, or unverifiable freshness and the reason
  that produced the overlay.
- Separate what the Wiki states from inference made for this answer. Preserve
  contradictory evidence and explain its scope.
- Prefer an explicit “the Wiki does not establish this” over filling a gap from
  memory.

If durable evidence is missing, name the gap and suggest the exact ingest or
research scope that could close it. Do not silently search `01_inbox`,
`02_Project`, `03_Area`, or `04_Resource` for a durable answer unless the user
asks to broaden the scope.

## Read-only boundary

A query does not update access logs, indexes, observation caches, manifests,
page status, or relationships. Do not promote material merely because it was
useful in an answer.

If the result has standalone long-term value, offer a separate
`wiki-synthesis` proposal. Saving requires the user's confirmation under the
knowledge-commit protocol.
