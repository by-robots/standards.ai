---
name: commit
description: Split the current changes into atomic commits and write Conventional Commit messages for them. Use when asked to commit work. Presents the plan for approval before staging or committing anything.
---

Split the current changes into atomic commits and write their messages.

**Usage:**
- `/standards-ai:commit` — commits all uncommitted changes, staged and unstaged
- `/standards-ai:commit <scope>` — commits only the files or directories at `<scope>`

## Instructions

1. Run `git status --short` and `git diff` (plus `git diff --cached` if anything is staged) to see all uncommitted work. If there is none, report that and stop.
2. Check the current branch. If it is the default branch, say so and ask whether to create a branch before continuing. Do not commit to the default branch without an explicit go-ahead.
3. Group the changes into atomic commits — one logical change each. A commit that changes behaviour and reformats unrelated files is two commits. A change split across several files is one commit, and all of those files belong in it.
4. Draft a Conventional Commit message for each group:
   - Type and description on the first line, imperative mood, no trailing full stop.
   - Add a body when the title alone does not explain why the change was made. State the problem the change solves, not a list of the edits — the diff already shows those.
   - Do not add co-author entries for AI tools.
5. Present the plan before touching the index: for each commit, the files it contains and the full message. If any change does not fit cleanly into a group, say so rather than forcing it in.
6. On approval, create the commits in order. Stage each group explicitly by path — never `git add -A` or `git add .`, which would sweep in files belonging to a later commit.
7. If a group cannot be staged separately because one file contains changes belonging to two commits, stop and report which file. Do not use `git add -p`; it needs the user.
8. Report the commits created, one line each: short hash and title. Do not push.
