---
name: ingest
description: Ingest new sources from raw/ into this wiki. Use when the user says ingest, process or add sources, or has put files in raw/. Follows the Ingest workflow, Index Structure and Version control rules in CLAUDE.md.
---

# Ingest

`CLAUDE.md` is the source of truth. This skill is the checklist; if the two disagree, follow `CLAUDE.md` and tell the user.

1. Run `git status`. If there are uncommitted changes, tell the user and offer to commit them first.
2. List the files in `raw/` (not `raw/archive/`). If there are none, say so and stop. Use shell globs for filenames with Unicode characters.
3. Read `wiki/INDEX.md`, `wiki/maintenance-decisions.md` (if it exists) and `PURPOSE.md`.
4. If there are several sources, show a table of each source and the pages it would create or update, and wait for confirmation. Do not write anything yet.
5. Read each source in full, then discuss the key takeaways with the user. Wait for their response before writing.
6. Write, following the Ingest workflow in `CLAUDE.md`: a summary in `wiki/summaries/`, updates to existing concept pages before any new ones, contradictions recorded (not smoothed), links only where the relationship is real, readable citations that link to `raw/archive/`.
7. Update `wiki/INDEX.md` per the Index Structure, append a `## [YYYY-MM-DD] ingest | Title` entry to `wiki/log.md` (create it if missing), and record rejected or deferred items in `wiki/maintenance-decisions.md`.
8. Move the processed sources to `raw/archive/`. Never edit their contents.
9. Check that every wikilink resolves.
10. Show `git status` and `git diff --stat`, summarise what changed, and ask for approval. Commit only after the user confirms, as one commit with the log heading as the message and the attribution line the session specifies. Never push.
