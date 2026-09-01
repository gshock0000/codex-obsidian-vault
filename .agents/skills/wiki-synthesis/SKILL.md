---
name: wiki-synthesis
description: Create a proposed durable comparison, report, or decision brief from confirmed Wiki evidence. Use when a query result deserves a reusable synthesis page.
---

# Wiki synthesis

Create a durable comparison, report, or decision brief from confirmed Wiki
evidence. Read the
[Wiki page contract](../vault-governance/references/wiki-page-contract.md), the
[Wiki state contract](../vault-governance/references/wiki-state-contract.md),
and the
[knowledge-commit protocol](../vault-governance/references/knowledge-commit-protocol.md).

Before assembling evidence, inspect `.state/write.lock` and
`.state/transactions/`. Do not draft or propose a new synthesis while a
knowledge/maintenance transaction is `preparing`, `prepared`, `applying`, or
`needs-recovery`. Report the pending/recovery state first; a partially applied
Wiki is not a valid synthesis baseline. A valid `observation_refresh` lock does
not change semantic files: ignore the cache being replaced and compute needed
local observations in memory.

## Evidence boundary

Synthesize from `06_wiki` resources and other evidence-linked Wiki pages, not
from recollection, a query answer alone, or live Project status. Every material
conclusion must cite at least one resource directly in `evidence_refs`.

If necessary evidence exists only in a source layer or external research:

1. identify the gap;
2. use `research-capture` when external research is needed;
3. use `wiki-ingest` to prepare the resource and any prerequisite page proposal;
4. synthesize only after that evidence is confirmed into the Wiki.

Do not smuggle unconfirmed material into a synthesis because it makes the report
more complete.

## Workflow

### 1. Frame the deliverable

State the question, intended use, scope, exclusions, audience, and `as_of` date.
Decide whether the answer deserves a durable artifact: it should combine
multiple evidence items, make a reusable comparison, resolve a recurring
question, or support a consequential decision. Otherwise answer with
`wiki-query` and do not create a page.

### 2. Assemble the evidence set

Use progressive retrieval, then read all pages that materially support,
contradict, or bound the synthesis. Inspect their review states and relevant
commit logs. List:

- accepted and provisional evidence;
- disputed or contradictory evidence;
- missing, archived, or changed dependencies;
- source independence and important coverage limits.

A `needs-review` resource may be discussed, but its affected conclusion and
freshness caveat must be explicit. A missing-source chain normally prevents
`confidence: high`.

### 3. Analyze transparently

Build a claim-to-evidence outline before drafting. For each major conclusion,
record supporting resource IDs, contrary evidence, inference steps, scope, and
uncertainty. Compare alternatives on explicit criteria rather than rhetorical
preference. Include the strongest supported objection and explain what evidence
would change the conclusion.

Use qualitative `low`, `medium`, or `high` confidence based on evidence
completeness, authority, independence, and contradiction. Do not calculate
confidence from link counts, page age, or model certainty.

### 4. Draft the page

Follow the synthesis body and frontmatter contract, including `as_of`,
`evidence_refs`, and `last_confirmed_commit`. Keep these sections distinct:

- source-backed evidence;
- analysis and inference;
- recommendations or decisions;
- strongest objection;
- unresolved questions.

Use path-qualified wikilinks and relationship edges only when their meaning is
explicit. A co-occurrence or graph proximity is a discovery hint, not evidence.

### 5. Choose update semantics

- Refine an existing synthesis only for clarification, new evidence that does
  not materially change its conclusion, or correction within the same dated
  scope.
- When later evidence materially changes the conclusion or recommendation,
  create a successor, add `supersedes` to the new page, and mark the old page
  `superseded` with `superseded_by`.
- Preserve a real disagreement as `disputed` or a scoped `contradicts` edge.
  Do not silently harmonize it.

## Proposal and commit

Prepare a bounded knowledge-commit proposal with the complete evidence set,
claim-to-evidence mapping, proposed text or section-level semantic diff,
confidence basis, objections, uncertainty, relationships, reserved commit ID,
operation time, evidence-page preconditions, proposal digest, target-table
digest, exact expected/desired hashes, and coupled index/manifest/log/overview
effects.

Stop for confirmation. After approval, use the transaction and validation
protocol. Do not update the Wiki, manifest, query logs, or index merely because
the synthesis draft was useful.
