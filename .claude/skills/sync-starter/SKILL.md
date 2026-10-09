---
name: sync-starter
description: Pull the latest shared scaffolding (CLAUDE.md, skills, folder layout) from the wiki-starter repository into this wiki without touching its content. Use when the user asks to update the schema, sync or merge the starter, or check for starter changes.
---

# Sync starter

This is a `schema` operation under the Version control rules in `CLAUDE.md`. Follow the "Keep the scaffolding up to date" steps in `README.md`.

1. Run `git status`. If the tree is not clean, tell the user and offer to commit first. Do not continue until it is.
2. Check `git remote -v`. If there is no `upstream` remote, ask the user for the starter's path or URL, add it as `upstream`, and tell them. If `origin` points at the starter, offer to rename it to `upstream`.
3. `git fetch upstream`. If there are no new commits (`git log HEAD..upstream/main --oneline` is empty), report that the wiki is up to date and stop.
4. Show the incoming commits and `git diff HEAD...upstream/main --stat`, then summarise in plain language what changes (schema rules, skills, layout). Flag anything that would affect existing pages, such as new frontmatter fields, renamed folders or changed log format.
5. Ask the user to approve the merge. Then run `git merge upstream/main` (no fast-forward-only, no rebase, no force).
6. On conflicts, list the files. For `CLAUDE.md`, show both versions of each conflicting block and ask which to keep; suggest moving wiki-specific wording into `PURPOSE.md`. Never resolve a conflict silently.
7. Append a `## [YYYY-MM-DD] schema | Sync with starter` entry to `wiki/log.md` listing what changed. Record any accepted, rejected or deferred schema change in `wiki/maintenance-decisions.md`.
8. If existing pages need migrating to match the new schema, propose that as a separate operation. Do not do it in this commit.
9. Show `git status` and `git diff --stat`, ask for approval, then commit (the merge commit, or a follow-up commit for the log entry). Never push.
