---
name: resource-management
description: Manage the user-curated resource library under 04_Resource, including source references, importance notes, and controlled handoff to Wiki resources.
---

# Resource management

`04_Resource` is the user's curated library: important links, documents, tools, and references worth keeping. It is distinct from `06_wiki/resources`, which contains agent-written, evidence-backed understanding pages.

## Resource records

Use a Markdown resource card when a resource needs context beyond its filename or URL. Keep a compact frontmatter with `id`, `type: resource`, `title`, `status`, `created`, `updated`, `tags`, and one or more `source_refs`.

For an external file, retain the original only once in `01_inbox/raw/` and reference its stable source ID. For a URL, record the canonical URL and capture metadata. Add personal notes explaining why the resource matters, its scope, and any related Project or Area.

Do not infer that every saved resource has been read, verified, or should become Wiki knowledge. Use statuses such as `saved`, `reviewed`, `used`, `stale`, or `archived`.

## Handoff to the Wiki

When the user asks to learn from a resource, use `wiki-ingest`. The proposal may create a `06_wiki/resources` page and then update concepts or entities, but it must preserve the resource's original source and must wait for user confirmation before writing the Wiki.
