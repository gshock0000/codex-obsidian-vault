# Project health refresh and validation

## Purpose and scope

Use this reference only for a selected project under `02_Project/`. The source
records are the project's plans, decisions, activities, and knowledge cards.
`overview.md` and weekly reports are derived context, never evidence that can
override those records.

This initial workflow provides two operations:

| Operation | User intent | Writes |
| --- | --- | --- |
| Validate | "validate/audit/review this project" | None. Report findings and proposed repairs separately. |
| Refresh overview | "refresh/update/rebuild the overview" | The derived parts of `overview.md` only. |

Do not create missing folders or records solely because validation finds them
absent. Do not introduce new frontmatter fields, task IDs, risk registers, or
tracker integrations.

## Source-reading order

1. Enumerate every Markdown file in the selected project folder. Record the
   scope and the check date.
2. Read `overview.md` as the prior derived view, then all files under
   `activities/`, including resolved records.
3. Read current plans and decisions, then the latest weekly report when one
   exists. Read knowledge cards only when they bear on an active item's
   evidence, risk, or decision.
4. For each statement in the result, link to the supporting source record.
   Prefer activity, plan, and decision records over a weekly report or prior
   overview.

If no project is selected, ask the user to name it. Never treat the repository
root as a project or scan unrelated project folders.

## Ongoing-activity assessment

Review every activity whose frontmatter has `status: ongoing`, regardless of
whether its `type` is activity, issue, test, or idea. Do not change its status
as part of this assessment.

For each record, identify the following from the record body and directly
linked current plan or decision:

| Signal | Evidence needed | Assessment rule |
| --- | --- | --- |
| Pending work | An unresolved outcome, next action, or concrete remaining step | Mark `clear` when present; otherwise mark `needs_clarification`. Do not infer an action merely from an old date. |
| Blocked or waiting | A named unmet dependency, external condition, approval, or explicit blocker | Mark the condition and record link. `blocked` requires explicit evidence that work cannot continue; a mere dependency is `waiting`. |
| Risk | An explicit concern, uncertainty, likely adverse effect, constraint, or unmitigated assumption | State the risk and its cited evidence. Age alone is only a review signal, never a risk. |
| Decision needed | An explicit choice, trade-off, approval request, or unresolved alternative that affects the work | State the decision and cited evidence. Do not relabel an already confirmed decision as pending. |

The signals overlap. Assign a concise primary posture for communication only:
`actionable`, `waiting`, `blocked`, `needs_decision`, `at_risk`, or `unclear`.
Use `unclear` when the source does not establish pending work or current state.
If several postures apply, choose the most immediate impediment and retain the
other signals in the explanation.

Never manufacture a risk, blocker, decision, owner, deadline, or completion
claim. Absence of evidence is an evidence gap, not proof that the work is safe
or complete.

## Validation checks

Report each finding as **structural** or **semantic/evidence**. Validation is
read-only.

### Structural checks

- expected project locations and Markdown files are readable; missing expected
  locations are reported but not created;
- editable records have the required frontmatter fields: `title`, `type`,
  `status`, `project`, `created`, `updated`, `tags`, `source`, and `summary`;
- `project` identifies the selected project and dates are valid, with
  `updated` not earlier than `created`;
- status values fit the record type as defined in `SKILL.md`;
- links that explicitly resolve to a file inside the selected project are not
  broken; external links and non-project Wiki links are not treated as local
  link failures;
- the overview's check date and derived counts are older than source updates
  only when the available dates establish that drift.

### Semantic and evidence checks

- every ongoing activity has the assessment above, especially a known pending
  item or a transparent `needs_clarification` result;
- completed-this-week AP counts have explicit completion evidence in a source
  record or the weekly report. A file's `updated` date by itself is not proof
  of completion;
- blockers, risks, and decisions in the overview have source links and no
  unsupported claims;
- overall health is `blocked` only with a documented material blocker;
  `at_risk` only with documented material risk or an unresolved decision that
  affects current work; and `on_track` only when the reviewed evidence supports
  it. Otherwise retain the prior health label and disclose the uncertainty.

## Refreshing the overview

When the user requests a refresh, use the overview template and update its
derived sections after completing the assessment.

1. State the check date and review scope.
2. Compute the AP and issue counts from source records. Use `unable_to_verify` rather
   than a fabricated number when the records cannot support a requested count.
3. Add the overlapping activity-signal counts: clear pending work, blocked or
   waiting, explicit risk, decision needed, and unclear.
4. List only exceptions and material items under blockers/risks, decisions, and
   unclear activities; link each to its source. Routine actionable records stay
   in navigation rather than being copied into the overview.
5. Derive current focus and upcoming plan items from active plans and activity
   evidence. Update overall health only under the evidence rules above.
6. Keep the latest weekly-report link and source navigation current.

If the existing overview contains clearly user-authored sections outside this
derived structure, preserve them. If it is ambiguous whether prose is derived,
show the proposed rewrite or ask before replacing it.

## Output for validation

Return a concise report containing:

1. review scope and check date;
2. structural findings, if any;
3. one assessment line for each ongoing activity: pending work, primary
   posture, risk/blocker, decision-needed, and source link;
4. semantic or evidence gaps;
5. proposed overview changes, separated from any proposed source-record
   repairs.

Do not make semantic repairs during validation. Ask for confirmation before
changing a source record. A refresh operation is authorization to update only
the derived overview.