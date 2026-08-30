---
name: area-management
description: Manage long-lived domain archives and evolving notes under 03_Area. Use for areas such as RF, personal learning, engineering practice, and their durable-but-editable notes.
---

# Area management

An Area is a user-maintained domain archive, not an agent-owned Wiki. It may contain persistent references, observations, methods, and continuously updated learning notes. Keep its structure understandable to the user; do not impose a universal taxonomy.

## Note modes

Declare an active note's synchronization mode in frontmatter when it will be considered for Wiki promotion:

```yaml
sync:
  mode: append # or snapshot
```

- `append`: for reading notes, research journals, and observation logs that normally grow at the end. After a confirmed Wiki commit, append a `<!-- wiki-commit: <id> -->` marker. Later ingest reads the tail first.
- `snapshot`: for methods, reference notes, and content that may be revised anywhere. Use content fingerprinting to detect changes and request review of affected Wiki resources.

The marker is a boundary for incremental reading, not proof that earlier text remains unchanged. If earlier content changes, mark dependent Wiki resources `needs-review`.

## Area metadata

For notes that need structured discovery, use `id`, `type`, `area`, `status`, `created`, `updated`, `tags`, and optional `entities`. Use a hierarchical area identifier such as `rf/antenna-design`; use tags only for topics, not ownership.

## Wiki boundary

Area files may yield candidate resources and concepts, but do not copy the Area into `06_wiki`. Preserve the Area page as context and provenance; compile only stable, reusable, or evidence-backed learning into the Wiki after user confirmation. Use `wiki-ingest` to prepare the proposal.
