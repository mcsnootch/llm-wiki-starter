# LLM Wiki

A personal knowledge base maintained by Claude Code.
## Purpose

Claude maintains the wiki. The human curates sources, asks questions, and guides the analysis.

This wiki's topic, scope and any local rules are in @PURPOSE.md. Read it first; do not edit the rest of this file for topic-specific needs.

## Folder structure

```
raw/             # source documents (immutable -- never modify these)
	archive/     # sources already ingested (move them here after ingest)
wiki/            # markdown pages maintained by Claude
	INDEX.md     # table of contents for the entire wiki
	concepts/    # concept pages (covering ideas, frameworks and methods)
	summaries/   # One summary page per raw/ file
	scenarios/   # scenario pages (domain specific use cases)
	syntheses/   # Cross-source synthesis pages
	qa/          # saved answers to questions (see Query)
	log.md       # append-only record of all operations
	maintenance-decisions.md # append-only record of review decisions (accepted, rejected, deferred, and why)
	
outputs/      # generated artifacts (reports, drafts), date-prefixed e.g. 2026-05-06_name.md
```

## Page Specifications

### Naming Conventions

All wiki page filenames use **Title-Case-Kebab**: `My-Concept-Name.md`

### Concept Pages (concepts/)

```yaml
---
type: concept
title:
tags: []
related: []
source_files: []     # Which raw/ files were used to generate this page
source_count: 0
last_updated:
confidence: high|medium|low
---

---

Main content goes here. Use clear headings and short paragraphs.

Link to related concepts using [[wiki-links]] throughout the text.

## Related pages

- [[related-concept-1]]
- [[related-concept-2]]
  
```

Must include: definition, core mechanism, applications and use cases, relationship to other concepts, source citations.

**Conflict rule:** When new source material contradicts existing content, record both positions, note the conflict explicitly, and set `confidence: low` until resolved.

**Length limit:** If a concept page exceeds ~200 lines, consider splitting into sub-concepts or promoting details to a synthesis page.

### Summary Pages (`summaries/`)

```yaml
---
type: summary
title:
source_files:
  - raw/...          # Path to the original source file
source_type: article|podcast|paper|note
date:
tags: []
---

---

Main content goes here. Use clear headings and short paragraphs.

```

Must include: key takeaways (3–5 bullets), list of key concepts (linked to `concepts/`), connections to existing wiki pages, link or path to the original source.

### Scenario Pages (`scenarios/`)

```yaml
---
type: scenario
domain: <your-domain>    # e.g., education, research, engineering
pain_points: []
source_files: []
---

---

Main content goes here. Use clear headings and short paragraphs.

Link to related concepts using [[wiki-links]] throughout the text.

## Related pages

- [[related-concept-1]]
- [[related-concept-2]]
  
```

> **Customize:** Define your own `domain` values that fit your knowledge base domain.

Must include: scenario description, key decision points, workflows and logic, references to existing implementations or patterns.

### Synthesis Pages (`syntheses/`)

```yaml
---
type: synthesis
title:
covers: []           # Concepts and entities referenced
source_files: []     # Which source files were synthesized
created:
confidence: high|medium|low
---

**Summary**: One to two sentences describing this page.

**Last updated**: Date of most recent update.

---

Main content goes here. Use clear headings and short paragraphs.

Link to related concepts using [[wiki-links]] throughout the text.

## Related pages

- [[related-concept-1]]
- [[related-concept-2]]

```

Must include: central argument, supporting evidence (cite specific wiki pages), contradictions with existing knowledge, hypotheses to verify, open questions.

### Index Structure (`wiki/INDEX.md`)

Order the sections as follows, so the index reads from ideas to sources:

1. **Concepts** (`##`): grouped under broad category sub-headings (`###`, e.g. Core concepts, Workflow and maintenance, Risks, Tools). Add a new category only when a page fits none of the existing ones.
2. **Scenarios**, **Syntheses** and **Q&A** (`##` each): added when the first page of that type exists.
3. **Source summaries** (`##`): always the last section, one entry per file in `summaries/`.

Each entry is `[[Page-Name|Readable Title]] — one-line description`.

## Operations
--- 
### Ingest workflow

When the user adds a new source to `raw/` and asks you to ingest it:

1. Read the full source document
2. Extract key information and discuss highlights and key takeaways with the user. Write nothing until they have responded.
3. Generate a summary page in `wiki/summaries/` and set `source_files` to the original path
4. Update relevant `concepts/` pages. Create or supplement existing pages. Create a new concept page only if it does not exist, and only if there is a clear source basis. Do not create a page just because a phrase sounds important.
5. When a source disagrees with an existing page, record the contradiction on the concept page, citing both sources. Do not smooth it into consensus.
6. Add wiki-links ([[page-name]]) to connect related pages. Link only where the relationship is real.
7. Update `wiki/INDEX.md` with new pages and one-line descriptions, following the Index Structure above
8. Append an entry to `wiki/log.md` in the log format below, with the source name and what changed
9. Move the processed source from `raw/` to `raw/archive/`

For multiple new sources, first present a table of each source and the pages it would create or update, and wait for confirmation before writing.

Filenames with Unicode characters (curly quotes, em dashes) may fail to read; use bash globs such as `cat "raw/"*"keyword"*`.

A single source may touch 10-15 wiki pages. That is normal.

---
### Query (Answer a Question)

1. First read `wiki/INDEX.md` to find relevant pages
2. Read the 2–3 most relevant wiki pages
3. Synthesize an answer — every important claim must cite its source wiki page or raw file
4. If the answer is valuable, save it to `wiki/qa/` as a new page (this step is mandatory), add it to `wiki/INDEX.md`, and log it as a `query` entry

--- 
### Generate Scenarios (Manual Trigger)

When the user says "generate a scenario page for [scenario name]":

1. Find all related summaries, concepts, and entities in `wiki/INDEX.md` and `wiki/`
2. Generate `wiki/scenarios/<scenario-name>.md`
3. Update `wiki/INDEX.md`
4. Append to `wiki/log.md` as a `scenario` entry, e.g. `## [YYYY-MM-DD] scenario | Scenario Name`, noting the number of source files in the bullets

---
### Generate Syntheses (Manual Trigger)

When the user says "generate a synthesis about [topic]":

1. Find all related concepts, entities, summaries, and qa pages
2. Generate `wiki/syntheses/<topic>.md`
3. Explicitly attribute each argument to its source page
4. Update `wiki/INDEX.md`
5. Append to `wiki/log.md` as a `synthesis` entry, e.g. `## [YYYY-MM-DD] synthesis | Topic`, noting the number of pages referenced in the bullets

---
### Lint (Health Check)

Before proposing fixes, read `wiki/maintenance-decisions.md` and do not re-propose rejected items. Check and append results to `wiki/log.md`:

- Orphaned pages (no incoming links)
- Dead links in `wiki/INDEX.md` (listed but file doesn't exist)
- Contradictory statements across pages
- Concepts mentioned repeatedly but lacking a dedicated page
- Missing summary pages for files in `raw/`

Report mechanical fixes (safe to apply, such as formatting or dead links) separately from editorial ones (rewording claims, merging or creating pages), and ask before applying editorial ones. Log format: `## [YYYY-MM-DD] lint | Title`, with the counts (e.g. orphaned pages) and issues found in the bullets. Record the outcome in `wiki/maintenance-decisions.md`.

---
## Citation rules

- Every factual claim should reference its source file
- Use human-readable citations that link to the source file: (source: [[raw/archive/filename|Author — Title (domain)]])
- For YouTube use Channel — Video Title; for forums use Reddit — r/subreddit thread
- If two sources disagree, note the contradiction explicitly
- If a claim has no source, mark it as needing verification

## Log format

Every `wiki/log.md` entry starts with a heading in this form, so the log can be searched with simple tools (e.g. `grep "^## \[" wiki/log.md | tail -5`):

```
## [YYYY-MM-DD] type | Title
```

Types: `ingest`, `schema` (changes to this file or conventions), `remove`, `lint`, `query` (answers filed back into the wiki), `scenario`, `synthesis`. Follow the heading with short bullets: pages created, pages updated, and anything left open.

## Maintenance decisions

`wiki/log.md` records what changed. `wiki/maintenance-decisions.md` records what was decided and why, including what was deliberately not changed, so later sessions do not re-propose settled items.

Add an entry after any lint, audit or clean-up, and after an ingest where something was rejected or deferred. Entry format:

- Date and task
- Pages reviewed
- Accepted changes
- Rejected changes, with the reason
- Deferred changes (e.g. a concept that lacks a source basis), with what would justify revisiting
- Reviewer notes

Rules:
- Read `maintenance-decisions.md` before proposing any maintenance change, and do not re-propose a rejected item unless the sources have changed.
- Keep maintenance changes small: propose at most three, apply the safest mechanical fix first, and ask before editorial changes.
- Never rewrite a claim or change a link target as a "mechanical" fix without the user's approval.
- The user decides accepted, rejected and deferred. Record their decision, not a guess.

## Version control

The vault is a Git repository. Commits mark the boundary between Claude's changes and the user's acceptance.

- Before an ingest, lint fix or schema change, check `git status`. If there are uncommitted changes, tell the user and offer to commit them first, so each operation starts from a clean checkpoint.
- After finishing, show a short summary (`git status` and `git diff --stat`) and ask the user to approve. Commit only after they confirm. Do not auto-commit.
- One commit per operation (one ingest, one lint pass, one schema change). Do not bundle unrelated changes.
- Commit message: the log heading, e.g. `ingest | Karpathy — LLM Wiki`, followed by the attribution line.
- Never push, force-reset or rewrite history without being asked. To undo an operation, use `git revert` or `git restore` after asking.
- Include the new log entry in the same commit as the work it records.
## Rules

- Never modify the contents of files in `raw/`; moving ingested files to `raw/archive/` is allowed
- Prefer updating existing concept pages over creating new ones when a topic is already covered
- Before creating any page, check the index for an existing page that covers the topic
- Split a page if it starts covering two concepts
- Always update `wiki/INDEX.md` and `wiki/log.md` after changes, and `wiki/maintenance-decisions.md` when a decision is made
- Write in clear, plain language
- When uncertain about how to categorize something, ask the user
