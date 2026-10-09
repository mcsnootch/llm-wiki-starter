---
name: new-wiki
description: Set up a freshly cloned wiki from the wiki-starter template. Use when the user says new wiki, set up this wiki, or when PURPOSE.md is still the unedited template.
---

# New wiki

Run this once, in a new clone of the starter.

1. Run `git status` and `git remote -v`. If `origin` points at the starter, offer to rename it to `upstream` (`git remote rename origin upstream`) so scaffolding updates can be pulled later. If the tree is not clean, tell the user.
2. Read `PURPOSE.md`. If it is no longer the template, ask whether to overwrite it or stop.
3. Ask the user, in one message, for: topic, audience, scope (in and out), and any extra rules for this wiki. Offer a draft if they are unsure.
4. Write `PURPOSE.md` with their answers. Do not edit `CLAUDE.md`; it is shared and merged from the starter.
5. Confirm the setup loaded: state the topic in one sentence from `PURPOSE.md`, and tell the user to check that Claude Code shows `PURPOSE.md` as part of its loaded instructions in a fresh session. If it is not picked up, tell the user so the starter can be fixed.
6. Create `wiki/log.md` with a first entry `## [YYYY-MM-DD] schema | Wiki created from starter`, listing the topic and starter commit (`git log --oneline -1 upstream/main` or `HEAD`). Fill in `last_updated` in `wiki/INDEX.md`.
7. Show `git status` and `git diff --stat`, ask for approval, and commit with the log heading as the message and the attribution line the session specifies. Never push.
8. Tell the user the next step: put a source in `raw/` and use `/ingest`.
