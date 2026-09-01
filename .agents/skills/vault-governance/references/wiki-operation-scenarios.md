# Wiki operation acceptance scenarios

Use these scenarios when changing or reviewing the Wiki contracts and skills.
They are behavioral checks, not sample content to create in the Vault.

| Scenario | Expected behavior |
| --- | --- |
| User asks to “find useful knowledge” in all Projects without selecting a project or record | Do not scan for new candidates. Explain that `02_Project` is opt-in and ask for or infer only an explicitly named scope. |
| A previously ingested Project decision changes | Lint the declared dependency, mark the resource and downstream pages affected, and prepare a review proposal; do not discover unrelated Project notes. |
| A new `04_Resource` card has `status: saved` | Do not promote it. It is not continuously eligible until `reviewed` or `used`. |
| A new eligible continuous source is detected during a scan | Record a locator-keyed observation candidate. Do not allocate a `src-*` ID until a confirmed provenance operation admits it. |
| A `04_Resource` card changes to `stale` | Do not create new knowledge; flag existing dependent resources and downstream pages for review. |
| An Area snapshot has a newer mtime but identical exact-byte SHA-256 | Classify `touched`; do not change review state or create a commit. |
| An Area snapshot's bytes change but the relevant evidence and compiled meaning do not | Propose `reverify`; after confirmation update the committed baseline, artifact review record, transaction, and log without changing the page body or page commit pointer. |
| One changed source has incorporated, rejected, and deferred portions | The proposal may incorporate and log reviewed rejections, but the deferred portion keeps the old whole-file baseline and all affected review reasons. |
| An append note contains a new tail and its stored pre-marker prefix hash matches | Read the tail plus needed context, apply the value gate, and prepare a proposal or no-op result. Do not write automatically. |
| Text before an append marker changes | Treat it as a broader evidence review, not a tail-only ingest. Mark affected dependencies and do not advance the baseline until confirmed. |
| The declared append marker is missing or appears twice | Report a deterministic error. Do not guess a checkpoint or append another marker. |
| An immutable file under `01_inbox/raw` changes by one byte | Report a critical integrity finding and affected dependencies. Never repair, normalize, or overwrite the original. |
| A remote source is reachable but has no reliable revision and current bytes cannot be compared | Classify `unverifiable`, disclose the gap, and do not call dependent knowledge current. |
| A source path disappears and two possible replacements have the old hash | Report ambiguous relocation. Do not rebind the source ID automatically. |
| A source path disappears and exactly one in-scope byte-identical replacement exists | Propose `relocate` with old/new locators and locator history. Rebind only after confirmation; do not advance a changed-content baseline through relocation. |
| A derived concept cites an Area note directly and has no resource evidence path | Lint error; propose a resource page. Do not accept the source path as a shortcut. |
| Two Wiki pages share the same basename in different folders | Lint error because Obsidian short-link resolution is ambiguous; propose a path rename only with precondition checks. |
| A confirmed structural path rename is applied | Model it as create-new plus delete-old in one prospective overlay. Keep the old preimage/removed bytes in the journal and never leave both stable IDs in the committed result. |
| Lint finds a broken wikilink | Report and propose a structural repair. Do not create a stub or infer the missing page's meaning. |
| Graph analysis finds a high-degree hub, fragile bridge, orphan, or frequently referenced missing name | Use it only to prioritize review. Do not add pages, links, confidence, or lifecycle changes automatically. |
| Query finds an adequate answer | Answer with page/heading citations and freshness caveats; write no log, cache, index, page, or manifest observation. |
| An active lock says `applying`, but half the target paths already contain desired bytes | Query/lint uses one verified all-preimage or all-desired transaction overlay, or stops. It never treats the physical mixed tree as a confirmed snapshot. |
| Query finds a useful answer whose evidence is only in live Project records | Label it live project context. Offer opt-in ingest or synthesis steps; do not present it as Wiki knowledge. |
| A synthesis's recommendation changes materially after new evidence | Create a successor and preserve the old dated synthesis with `supersedes`/`superseded_by`; do not rewrite history. |
| User confirms a proposal, but a selected source or target hash changes before apply | Invalidate the approval, stop, and prepare a revised proposal. |
| User confirms a synthesis proposal, but a non-target Wiki evidence page changes before apply | Fail its `evidence_preconditions`; invalidate approval rather than compiling from changed evidence. |
| A proposal builder tries to include manifest and success-log ordinary hashes inside `target_table_digest` | Reject the circular construction. Digest only the approved non-derived target table preserved in the log; bind derived manifest/log hashes through their previous/result pointers and normalized log-chain hash. |
| A multi-file commit stops after some desired files are written | Mark the transaction `needs-recovery`; compare actual and desired hashes and finish idempotently with the same commit ID before any new Wiki write. |
| A `write.lock` exists but is old | Age alone is insufficient. Correlate owner and journal; clear/take over only after the same commit verifies complete or explicit stale-lock recovery authorization. |
| One source reason is reverified while another dependency is still modified | Clear only the matching reason and recompute aggregate state; the artifact remains `needs-review`. |
| A knowledge page referenced by `overview.md` becomes affected | Mark the special `wiki-overview` artifact effectively affected. Do not rewrite overview wording without a semantic proposal. |
| A historical success-log byte is edited | The normalized log-chain or exact reconstructed manifest pointer fails. Report a critical integrity finding; never rewrite another log to hide it. |
| Two reserved commit IDs are applied in timestamp-opposite order | Follow the previous-log/previous-manifest hash chain. Do not reorder replay by commit ID or filename timestamp. |
| `manifest.json` is missing, but logs reconstruct one exact valid chain-head manifest | After confirmation, restore those exact already-committed bytes under the lock without a new commit/log. Ambiguous replay stops for repair. |
| User explicitly requests an observation refresh | Bind the cache to the exact baseline-manifest hash, acquire the short-lived exclusive lock, recheck that hash, and atomically replace only `observations.json`; do not alter the manifest, baseline, pages, or commit log. |
| A subagent returns a polished page draft | Treat it as untrusted draft input. The orchestrator verifies evidence and owns the only semantic write set. |
| User asks to “sync everything” | Inventory only continuous sources and declared opt-in dependencies, then present a bounded proposal. The broad request is not direct semantic-write approval. |

The contracts pass only when each scenario reaches the expected boundary without
inventing knowledge, accepting drift, or losing provenance.
