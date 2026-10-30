---
name: update-readme
description: Sync the GitHub profile README.md with the current public repos under wmar-dev. Use when asked to update, refresh, or sync the README / profile.
---

# Update profile README

Keep `README.md` in sync with the public, non-fork repos owned by `wmar-dev`.

## Steps

1. List repos:
   ```sh
   gh repo list wmar-dev --limit 200 --json name,description,isFork,isArchived,isPrivate,primaryLanguage \
     --jq '.[] | [.name, (.isPrivate|tostring), (.isFork|tostring), (.isArchived|tostring), (.primaryLanguage.name // ""), .description] | @tsv'
   ```
2. Candidates = public, not a fork, not archived, not in the exclusion list below.
3. Compare against the repo links already in `README.md`:
   - **Missing** → add a row to the best-fitting section table.
   - **Listed but gone** (deleted, now private, archived, or excluded) → remove the row.
   - Leave existing rows and descriptions alone unless the repo was removed.
4. Row format: `| [name](https://github.com/wmar-dev/name) | Description |`
   - Tidy the GitHub description: fix typos, sentence case, drop the trailing period.
   - Place related repos next to each other (e.g. LLM wiki repos together).
   - If no section fits, add a new `## <emoji> Title` section with a table, before "🧰 Tech I Use".
5. If a newly added repo's primary language has no badge under "🧰 Tech I Use", add a shields.io badge matching the existing style.
6. Show the diff and summarize what was added and removed. Don't commit unless asked.

## Exclusions

Never list these, even though they're public:

- `wmar-dev` (this profile repo)
- `llm-wiki-hn`
