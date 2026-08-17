---
name: sync
description: Update this project's standards.ai template files to a newer version and report what changed. Use when asked to update, sync, or upgrade the CLAUDE.md rules or the standards-ai skills and agents.
---

Update this project's standards.ai template files from a newer copy of the
template repository.

**Usage:**
- `/standards-ai:sync <path>` — path to a local checkout of the standards.ai repository
- `/standards-ai:sync` — asks for the path if it is not already known

## Instructions

1. Determine the source. If `$ARGUMENTS` names a path, use it. Otherwise ask for the path to a local checkout of the standards.ai repository. Confirm it contains `templates/CLAUDE.md`; if not, report that and stop.
2. Read the first line of this project's `CLAUDE.md` and of the source's `templates/CLAUDE.md` to get the current and incoming version markers. If either has no marker, treat it as pre-1.1.0.
3. If the versions match, report that the project is up to date and stop.
4. Read `CHANGELOG.md` in the source repository and list the entries between the two versions. Call out any "Migrating from" note that applies — those describe work the file copy cannot do.
5. Diff the incoming files against the project's current copies:
   - `CLAUDE.md`
   - `.claude/conventions.md`
   - `.claude/skills/standards-ai/`

   Do not diff `.claude/project.md` or `.claude/review-violations.md`. Those hold the user's own content and are never overwritten.
6. Report, before changing anything:
   - The version change.
   - The changelog entries that apply.
   - For each file, whether it is unchanged, updated, or new.
   - **Any local modification that would be lost.** Compare each project file against the *old* version of the same file in the source repository (`git show <old-version-tag>:templates/...` if the source is a git checkout). Anything differing is a local edit that overwriting will discard — list it and quote it. This is the one thing the user cannot recover afterwards, so do not summarise it, show it.
7. Ask for confirmation. If any local modification was found, ask specifically what to do with it before proceeding.
8. On approval, copy the approved files over. Do not touch `project.md` or `review-violations.md`.
9. Apply any migration steps the changelog describes, presenting each one for approval separately — they change content the user wrote.
10. Report what was copied, what was skipped, and any migration step left for the user to do by hand.
