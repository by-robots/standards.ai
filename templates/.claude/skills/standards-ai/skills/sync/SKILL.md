---
name: sync
description: Update this project's copied standards.ai rule files to a newer version and report what changed. Use when asked to update, sync, or upgrade the project's rules from a standards.ai checkout.
---

Update this project's standards.ai template files from a newer copy of the
template repository.

**Usage:**
- `/standards-ai:sync <path>` — path to a local checkout of the standards.ai repository
- `/standards-ai:sync` — asks for the path if it is not already known

## Instructions

1. Determine the source. If `$ARGUMENTS` names a path, use it. Otherwise ask for the path to a local checkout of the standards.ai repository. Confirm it contains `templates/CLAUDE.md`; if not, report that and stop.
2. If `.claude/rules` is a symlink, this project tracks the checkout directly and there is nothing to copy. Report that, confirm the symlink target matches the source, and stop.
3. Find the version marker in this project's `CLAUDE.md` and in the source's `templates/CLAUDE.md` — grep for `standards-ai template`, do not assume it is the first line, since the template may have been appended to an existing file. If either has no marker, treat it as pre-2.0.0.
4. If the versions match, report that the project is up to date and stop.
5. Read `CHANGELOG.md` in the source repository and list the entries between the two versions. Call out any "Migrating from" note that applies — those describe work the file copy cannot do.
6. Diff the incoming files against the project's current copies:
   - `CLAUDE.md` — compare only the template block, from the version marker to the end of the imports and precedence text. If the project has its own content above or below that block, it is not part of the template and must survive: replace the block in place rather than copying the file over.
   - `.claude/rules/` — compare file by file, and list rule files that are new in the source or no longer present in it
   - `.claude/skills/standards-ai/`, if the project has it. Skip it if the plugin is installed from a marketplace instead.

   Do not diff `.claude/project.md` or `.claude/review-violations.md`. Those hold the user's own content and are never overwritten.
7. Report, before changing anything:
   - The version change.
   - The changelog entries that apply.
   - For each file, whether it is unchanged, updated, new, or removed upstream.
   - **Any local modification that would be lost.** Compare each project file against the *old* version of the same file in the source repository: `git show v<old-version>:templates/...`, falling back to `git log -S'standards-ai template v<old-version>' -- templates/CLAUDE.md` to find the commit if that tag does not exist. If neither resolves, say the comparison could not be made rather than assuming there are no local edits. Anything differing is a local edit that overwriting will discard — list it and quote it. This is the one thing the user cannot recover afterwards, so do not summarise it, show it.
   - Any `paths:` frontmatter the project has tuned to its own layout. Overwriting a scoped rule file silently reverts those globs, which stops the rule matching anything without producing an error.
8. Ask for confirmation. If any local modification was found, ask specifically what to do with it before proceeding.
9. On approval, copy the approved rule files over, and replace the template block within `CLAUDE.md` rather than overwriting the whole file. Do not touch `project.md` or `review-violations.md`. Never delete a rule file the project has that the source does not — report it and let the user decide.
10. Apply any migration steps the changelog describes, presenting each one for approval separately — they change content the user wrote.
11. Report what was copied, what was skipped, and any migration step left for the user to do by hand.
