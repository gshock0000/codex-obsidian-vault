---
name: research-capture
description: Capture externally researched material into 01_inbox before any Wiki promotion. Use when the user asks for web or external research that may be retained in the knowledge vault.
---

# Research capture

External research must enter the vault through `01_inbox`, never directly through `06_wiki`.

## Capture rule

- Preserve retrieved source material, page captures, downloaded files, or source metadata under `01_inbox/raw/` without editing the original content.
- Save the agent's generated research report under `01_inbox/research/` or `01_inbox/report/`, with source references and the research date.
- Distinguish quoted or observed source facts from the agent's inference. Do not present a report as an original source.
- Give a generated report `ai_generated: true` and an appropriate `status` such as `done`. Record its sources and date in the normal note fields. Let the ingest manifest issue stable source IDs only if the material is promoted to the Wiki.

## Promotion rule

Research reports and captures become candidates for `wiki-ingest` only after the user requests or confirms a knowledge promotion. Do not create or update `06_wiki` merely because a research task completed.
