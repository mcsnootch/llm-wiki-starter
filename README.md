# Wiki Starter

A template for LLM-maintained wikis (the Karpathy "LLM Wiki" pattern) in Obsidian, maintained by Claude Code. Clone it once per topic. Each clone gets its own wiki content, and the shared maintenance scaffolding (the schema and folder layout) can be updated from this repo later.

## What is in the repo

```
CLAUDE.md        shared schema: folders, page formats, ingest/query/lint workflows, log, Git rules
PURPOSE.md       per-wiki topic, scope and local rules (imported by CLAUDE.md; replace in each clone)
raw/archive/     source documents; ingested sources move to archive/
wiki/            INDEX.md plus concepts/, summaries/, scenarios/, syntheses/, qa/
outputs/         generated artifacts
```

`wiki/log.md` and `wiki/maintenance-decisions.md` are not included. Claude creates them on the first operation.

Ownership rule: **`CLAUDE.md`, `README.md` and the folder layout belong to the starter.** Everything a wiki needs to be about its own topic goes in `PURPOSE.md` and the wiki content. This keeps later merges conflict-free.

## Create a new wiki

1. `git clone path/to/wiki-starter my-topic-wiki`
2. `cd my-topic-wiki`, then point the remotes: `git remote rename origin upstream`. Add your own `origin` if the wiki will have a remote.
3. Edit `PURPOSE.md`: topic, audience, scope, extra rules.
4. Open the folder as an Obsidian vault and start Claude Code in it. Check that Claude has read `PURPOSE.md` (the `@PURPOSE.md` import in `CLAUDE.md` should load it).
5. Drop a source into `raw/` and ask Claude to ingest it. Commit when you approve the result, as `CLAUDE.md` describes.

## Keep the scaffolding up to date

Run this in a wiki when the starter has changed. It is a `schema` operation under the Version control rules.

1. Check `git status` in the wiki. Commit or stash anything outstanding, so the merge starts from a clean checkpoint.
2. `git fetch upstream`
3. Review what changed: `git log HEAD..upstream/main --oneline` and `git diff HEAD...upstream/main`.
4. `git merge upstream/main`. Content files (`raw/`, `wiki/`) are not in the starter, so they are untouched.
5. Resolve any conflict in `CLAUDE.md` by hand. A conflict means the wiki edited shared schema text; prefer moving that wording into `PURPOSE.md` so it does not recur.
6. Ask Claude to add a `schema` entry to `wiki/log.md` (and to `wiki/maintenance-decisions.md` if you accepted, rejected or deferred any of the schema changes), then review `git diff --stat` and commit.
7. If the schema changed in a way that affects existing pages (for example new frontmatter fields or a new folder), ask Claude to propose a migration as a separate commit. Do not mix it with the merge.

## Improve the starter

1. Make and test the schema change in a wiki where it came up.
2. Copy the change into the starter's `CLAUDE.md`, keeping wiki-specific text out. Remove anything topic-specific.
3. Commit it in the starter with a clear message, e.g. `schema | Ingest step order`.
4. Run the update steps above in the other wikis when you want the change.

## Notes

- Create wikis with `git clone`. A plain copy has unrelated history and needs `git merge --allow-unrelated-histories`.
- Never push to or rewrite the starter's history from a wiki.
- Each clone's `.gitkeep` files can be deleted once the folders have content.
