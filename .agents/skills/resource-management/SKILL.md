---
name: resource-management
description: Manage the user-curated resource library under 04_Resource, including source references, importance notes, and controlled handoff to Wiki resources.
---

# Resource management

`04_Resource` is the user's curated library: important links, documents, tools, and references worth keeping. It is distinct from `06_wiki/resources`, which contains agent-written, evidence-backed understanding pages.

## Resource records

Use a Markdown resource card when a resource needs context beyond its filename or URL. Keep a compact frontmatter with `title`, `type: resource`, `status`, `area`, `created`, `updated`, `tags`, `source`, and `summary`. Do not require manually maintained IDs or Wiki tracking fields.

For an external file, retain the original only once in `01_inbox/raw/` and point `source` to its local path. For a URL, use `source` for the canonical URL. The manifest issues a stable source ID only when needed for Wiki provenance. Add personal notes explaining why the resource matters, its scope, and any related Project or Area.

Do not infer that every saved resource has been read, verified, or should become Wiki knowledge. Use the required `status` field with values such as `saved`, `reviewed`, `used`, `stale`, or `archived`.

## Handoff to the Wiki

Resource is a continuous Wiki source. Treat only cards with `status: reviewed`
or `used` as new promotion candidates during normal sync; `saved` is not yet
eligible. A `stale` or `archived` card may still affect an already-declared
dependency but does not create new knowledge. Use `wiki-ingest` to prepare a
proposal without waiting for another ingest request. The proposal may create a
`06_wiki/resources` page and then update concepts, entities, claims, or
syntheses, but it must preserve the resource's original source and wait for
user confirmation before writing the Wiki.
