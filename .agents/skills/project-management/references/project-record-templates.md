# Project record templates

Use these only when the user asks the agent to create a project record. They
are agent-internal references, not Obsidian templates. Keep the required
frontmatter fields; omit empty optional body sections rather than inventing
content.

## Plan

```markdown
---
title: "<plan title>"
type: plan
status: ongoing
project: <project>
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
tags: []
source:
summary:
---

## Objective and Completion Criteria

## Scope and Boundaries

## Milestones or Steps

## Dependencies, Assumptions, and Risks

## Evidence and Updates
```

## Decision

```markdown
---
title: "<decision title>"
type: decision
status: ongoing
project: <project>
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
tags: []
source:
summary:
---

## Decision

## Context and Evidence

## Alternatives Considered

## Impact and Follow-up Actions

## Supersession
```

Use `status: done` only after the decision is confirmed. Record a later
replacement as `superseded`; do not erase its history.

## Activity, issue, test, or idea

```markdown
---
title: "<record title>"
type: <activity | issue | test | idea>
status: ongoing
project: <project>
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
tags: []
source:
summary:
---

## Pending Work

## Current Status and Evidence

## Blockers, Waiting Conditions, or Risks

## Decisions Needed

## Next Action
```

For every new `ongoing` record, state the unresolved work under `Pending Work`
or `Next Action`. If there is no known blocker, risk, or decision needed, omit its
body section or say that none is currently known; never manufacture one. Tests
may add `## Method and Results`; ideas may add `## Hypotheses and Exploration`.

## Knowledge card

```markdown
---
title: "<knowledge title>"
type: knowledge
status: ongoing
project: <project>
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
tags: []
source:
summary:
---

## Source References

## Project-specific Use

## Scope Boundaries or Open Questions
```

Link original material by local path or URL. Do not copy an external original
into the knowledge card.