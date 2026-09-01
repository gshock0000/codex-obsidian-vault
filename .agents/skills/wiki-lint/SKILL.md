---
name: wiki-lint
description: Audit structure, provenance, freshness, and dependency drift in 06_wiki without silently changing knowledge. Use when the user asks for Wiki health, synchronization, stale-content, or link checks.
---

# Wiki lint

Audit the compiled Wiki and its declared dependencies. Default to read-only:
compute findings in memory and do not update pages, logs, navigation, source
markers, or manifest observations.

## Load the contracts

Read:

1. [Wiki page contract](../vault-governance/references/wiki-page-contract.md);
2. [Wiki state contract](../vault-governance/references/wiki-state-contract.md);
3. [Knowledge-commit protocol](../vault-governance/references/knowledge-commit-protocol.md)
   when proposing repairs.

## Scope

Enumerate every Markdown page under `06_wiki`, plus `.state/manifest.json`,
`.state/observations.json`, and
`.state/write.lock` and complete
`.state/transactions/<operation-id>/` records when present. Do not trust
`index.md` as a complete inventory.

For source checks:

- inspect all eligible `03_Area` and `04_Resource` records for continuous-source
  metadata and change candidates;
- inspect `01_inbox` and `02_Project` only when a manifest dependency already
  exists; never discover new opt-in promotions;
- resolve existing provenance into `05_Archived` but never treat archived
  material as a new candidate.

State the exact scope, time, skipped files, inaccessible remote sources, and any
bounded semantic sample.

## Phase A: deterministic checks

### Pages and frontmatter

- Classify the inventory before validation: apply the five knowledge-page
  schemas only under their five folders; apply the separate special schema to
  `overview.md`, the deterministic grammar to `index.md`, and the commit-log
  schema under `log/`. Do not demand knowledge-page frontmatter from navigation
  or operational files.
- Parse complete YAML frontmatter and required type-specific fields.
- Check ID syntax and global uniqueness; `type` versus folder; lifecycle status;
  qualitative confidence; ISO dates; summary length; origins shape; and
  `last_confirmed_commit`.
- Require single-line title/summary without reserved Wiki-link delimiters and
  enforce basename uniqueness by Unicode-NFC casefold.
- Require resource `source_refs`; require derived `evidence_refs`; require
  `as_of` for syntheses and `superseded_by` for superseded pages.
- Detect duplicate basenames, empty/stub pages, and page paths outside the
  allowed direct V1 Wiki type folders.

### Evidence and relationships

- Resolve every source ID, evidence ID, relationship target, and supersession.
- Require every derived `evidence_refs` item to resolve directly to a resource
  page and every material conclusion to cite at least one such resource.
- Accept only `derived_from`, `supports`, `refines`, `contradicts`, `applies_to`,
  `uses`, and `supersedes`, with the direction defined by the page contract.
- Rebuild `depends_on` from resource source refs, derived evidence refs, and
  `derived_from`/`uses` targets. Include `wiki-overview` as a special artifact
  depending on every knowledge page in its evidence refs; verify `downstream`
  as the exact inverse.
- Detect dangling, self, duplicate, and impossible edges. Treat a
  `derived_from` dependency cycle as an error and a `uses` dependency cycle as
  a warning. Do not reject cycles formed only by other semantic relationship
  types, manufacture reciprocal links, or infer a relationship from
  co-occurrence.

### Links and navigation

- Parse wikilinks and Markdown links while ignoring fenced/inline code.
- Resolve path-qualified links, headings, and aliases; report ambiguous
  short-name links even if Obsidian happens to choose one target.
- Verify that `index.md`, when present, lists accepted/provisional/disputed
  pages exactly once in their type section and archived/superseded pages exactly
  once in `History`, uses the exact entry shape, and follows canonical sorting.
- Validate `overview.md` frontmatter, evidence IDs, and links when it exists.
  Require its special manifest artifact and reconstructed dependencies. Treat it
  as a selective view, not an inventory. Deterministically report affected
  freshness in Phase A; review unsupported or semantically stale wording only
  in Phase B.

### Logs, transactions, and state

- Inspect the exclusive lock before trusting the visible tree. Correlate it with
  its journal and owner; age alone never makes it stale. If an operation is
  `applying` or `needs-recovery`, audit either the verified all-preimage or
  all-desired overlay and never mix it with the physical partial tree. A valid
  `observation_refresh` lock has no transaction journal: ignore the cache being
  replaced and compute observations in memory while auditing semantic files.
- Validate lock JSON, operation class/ID, canonical vault root, applicable
  digests, owner metadata, and its journal correlation. Reject a lock or target
  path that resolves outside the vault through traversal, symlink, or junction.
- Resolve every `last_confirmed_commit` to exactly one append-only log and every
  manifest commit entry to that log. Recompute normalized verifiable log hashes,
  approved-target-table digests, and exact derived manifest/log hashes;
  follow the unique previous-log/previous-manifest chain rather than timestamps,
  and verify manifest `log_head` plus every exact result-manifest hash. A fork,
  gap, unreferenced success log, rewritten byte, or non-canonical manifest is an
  integrity finding.
- Resolve each success log to its retained committed `transaction.json`;
  canonically recompute `proposal_digest` from the stored approval core and
  compare the complete derived execution table. A missing transaction is an
  approval-audit gap even though manifest replay still uses logs alone.
- Compare page/source/action records in logs with the current files and
  manifest. If a missing/corrupt manifest reconstructs to one exact chain-head
  result, report `manifest_restore`; ambiguous replay is not repairable state.
- Validate manifest schema, source and artifact maps, locator history,
  committed baselines, confirmed review reasons, and stored dependency edges.
- Validate the optional observations-cache schema, scan scope/cutoff,
  locator-keyed new candidates, known-source relocation candidates, and
  effective-review overlay. Require its `baseline_manifest_sha256` to match the
  current exact manifest or ignore the cache. A new candidate must not receive a
  stable source ID before confirmation. Never mistake observations for a
  committed baseline.
- Rebuild the dependency mapping in memory and compare it with the manifest.
- Detect `preparing`, `prepared`, `applying`, or `needs-recovery` transactions
  and report which expected/desired/deleted/removed target states match; do not
  start a new Wiki write.

### Source freshness

- Validate required metadata on synced Area and Resource notes: `title`, `type`,
  `status`, `created`, `updated`, `tags`, `source`, `summary`, and `area`.
- Resolve local paths inside the vault. Use mtime and size only to shortlist,
  hash exact bytes with SHA-256, and compare with the committed baseline.
- Apply the state contract's `unchanged`, `touched`, `modified`, `new`,
  `missing`, `moved`, and `unverifiable` classifications.
- Report a unique byte-identical replacement path as a relocation candidate,
  not an automatic rebind. Report multiple candidates as ambiguity; locator
  history changes only through confirmed `relocate` maintenance.
- For append sources, verify the exact declared marker and prefix fingerprint
  before inspecting the tail. Report missing/duplicate markers and pre-marker
  edits as errors.
- Treat any changed immutable raw original as a critical integrity finding.
- For remote sources, compare a reliable revision or ETag when available and
  never call an unversioned source current merely because retrieval succeeded.
  Perform network freshness retrieval only when the user requested that scope;
  otherwise report the cached cutoff or `unverifiable`.
- Map direct source findings to resources, then propagate affected review state
  transitively without changing page semantic status. Report confirmed and
  effective review state separately and preserve reasons per dependency.

## Phase B: bounded semantic review

Clearly label this phase as evidence-based judgment, not deterministic lint.
Record which pages and sections were read. Review, as scope permits:

- material statements that lack reachable evidence;
- candidate contradictions or scope conflicts;
- duplicate or substantially overlapping pages;
- summary/body disagreement;
- unsupported or semantically stale `overview.md` statements;
- confidence or lifecycle language unsupported by the recorded evidence;
- syntheses whose evidence changed or whose strongest objection is absent;
- graph gaps, phantom hubs, fragile bridges, or orphans worth human review.

Graph metrics and age may prioritize inspection, but they do not prove stale
meaning, justify a new edge, change confidence, or promote lifecycle status.
Never run an LLM against only an arbitrary first-N slice and report the result as
a complete semantic audit.

## Report

Use `critical`, `error`, `warning`, and `info` severities. Reserve `critical` for
raw-original integrity changes, a broken manifest/log chain, or a transaction
conflict that cannot be resolved from approved expected/desired state. For every
finding include: finding ID, phase, affected path/ID, observed fact, evidence,
impact chain, and a specific next action. Separate:

1. structural/provenance facts;
2. freshness classifications and affected downstream pages;
3. semantic review candidates and coverage limits;
4. proposed deterministic repairs;
5. semantic changes that require a knowledge-commit proposal.

Do not write a lint log merely because lint ran. Return the report in chat by
default; save it under `01_inbox/report/` only when the user asks.

## Repair boundary

Lint never overwrites a source or Wiki page. Present safe structural fixes as an
exact, independently confirmable set. A broken target does not authorize a stub;
an orphan may be intentional. Any repair that changes meaning, evidence
assessment, confidence, status, or relationship substance follows the full
knowledge-commit protocol.
