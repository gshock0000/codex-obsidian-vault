---
name: area-management
description: Manage long-lived domain archives and evolving notes under 03_Area. Use for areas such as RF, personal learning, engineering practice, and their durable-but-editable notes.
---

# Area management

An Area is a user-maintained domain archive, not an agent-owned Wiki. It may contain persistent references, observations, methods, and continuously updated learning notes. Keep its structure understandable to the user; do not impose a universal taxonomy.

## Note modes

Keep Area note YAML limited to `title`, `type`, `status`, `area`, `created`, `updated`, `tags`, `source`, and `summary`. Use `status` for the note lifecycle, such as `ongoing`, `done`, or `archived`. When a note is considered for Wiki promotion, `wiki-ingest` may add a synchronization mode:

```yaml
sync:
  mode: append # or snapshot
```

- `append`: for reading notes, research journals, and observation logs that normally grow at the end. After a confirmed Wiki commit, append a `<!-- wiki-commit: <id> -->` marker. Later ingest reads the tail first.
- `snapshot`: for methods, reference notes, and content that may be revised anywhere. Use content fingerprinting to detect changes and request review of affected Wiki resources.

The marker is a boundary for incremental reading, not proof that earlier text remains unchanged. If earlier content changes, mark dependent Wiki resources `needs-review`.

## Area metadata

Use a hierarchical area identifier such as `rf/antenna-design`; use tags only for topics, not ownership. Do not add manually maintained IDs or Wiki tracking fields to ordinary Area notes.

## Wiki boundary

Area is a continuous Wiki source. Detect Area changes during normal sync and use `wiki-ingest` to prepare a proposal without waiting for an explicit ingest request. Do not copy an Area into `06_wiki`: compile only stable, reusable, or evidence-backed learning, preserve its provenance, and wait for user confirmation before semantic Wiki writes.
