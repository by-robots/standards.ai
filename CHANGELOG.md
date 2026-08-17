# Changelog

Notable changes to the templates. Each released version is stamped in the
first line of `templates/CLAUDE.md`, so you can tell which version a project
last received.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and
versioning follows [Semantic Versioning](https://semver.org/):

- **Major** — a change requiring manual work in projects already using the
  template, such as moving content between files.
- **Minor** — new rules, skills, or agents.
- **Patch** — wording, clarifications, and fixes that do not change behaviour.

## [Unreleased]

### Added

- `evals/` — a first `claude plugin eval` suite of five cases covering the
  review-routing boundary, the commit and fix-bug workflows, and the two new
  security rules, plus `scaffold.sh` to build each case's working directory.
  Not yet run: `plugin eval` is in early access and was not enabled on the
  account it was written on. See `evals/README.md` for what is verified and
  what is inferred.

## [2.0.0] — 2026-08-17

### Added

- `.claude-plugin/marketplace.json` at the repository root, making this repo a
  plugin marketplace. The skills and agents install once at user scope instead
  of being copied into each project, and update with `git pull` plus
  `/plugin marketplace update`.
- `.claude/rules/` — the rules are now fifteen topic files instead of one
  `CLAUDE.md`. Eight load every session; seven carry `paths:` frontmatter and
  load only when Claude touches a matching file, so a TypeScript project no
  longer pays for the Ruby, database, CSS and mark-up rules on every turn.
  Files are numbered to fix the read order, most critical first. Security,
  communication and the general conventions are deliberately unscoped:
  path-scoped rules are not re-injected after `/compact`.
- Rules previously in `project.md` — Communication Style, Commit Style and
  Language Conventions — are now rule files. They were living in the one file
  that copy-over updates deliberately skip, so no improvement to them could
  ever reach a project already set up.
- **Project Overrides** section in `project.md` for recording deliberate
  departures from the shared rules, and a precedence rule in `CLAUDE.md`
  stating that project rules win.
- A version marker on the first line of `CLAUDE.md`, an `author` field in
  `plugin.json`, and this changelog.
- Two documented ways to get the rules into a project: copy `.claude/rules/`,
  or symlink it to a checkout so `git pull` updates every project at once.
  Each has install and update instructions and the trade-off is stated.
- Security rules covering three gaps: printing secret values into the
  transcript, treating fetched or third-party content as instructions, and
  outward-facing operations (push, deploy, publish, remote migrations) that
  the existing destructive-operations rule did not reach.
- Git rules against committing unprompted and against committing directly to
  the default branch.
- Claude Code rules against creating unrequested files and against starting
  long-running processes without asking.
- `/standards-ai:commit` — splits changes into atomic commits and writes
  Conventional Commit messages, with approval before staging. Pairs with
  `preflight`, which checks the same changes but commits nothing.
- `/standards-ai:sync` — updates a project's copied rule files from a local
  checkout, reporting the changelog entries between the two versions and
  quoting any local edit the copy would overwrite.

### Changed

- "Follow existing patterns in the codebase" and "Follow the established
  style guide" are replaced by one testable rule: read a comparable file
  first and match it, or say why you deviated. Neither original could be
  checked by reading the output.
- The rule against removing code now exempts refactor, cleanup and removal
  tasks, where the blanket version guaranteed dead code accumulated and
  every session ended with a manual approval list.
- Testing rules are their own file rather than being buried among the
  language and framework rules. No wording changed.
- `review`, `preflight` and the `code-reviewer` agent now state their
  boundaries in the descriptions the model routes on, so an ambiguous
  "review my changes" no longer picks one at random. The agent asks which
  the user wants instead of defaulting to the expensive path.
- `review` checks each file only against the rules whose `paths:` frontmatter
  matches it, rather than sweeping every rule against every file, and asks
  before reading more than 20 files. This addresses the token cost the README
  previously only warned about.
- `code-reviewer` and `system-architect` no longer pin `model: opus`. A
  hardcoded model name ages badly in a template distributed across projects.
  Both inherit the session's model; a comment explains how to pin one.

### Fixed

- The documented install command nested a second directory inside an existing
  one, and copied `project.md` before anything created `.claude/`. The
  sequence now creates the directory first and uses `cp -n` so a
  hand-written `project.md` is never overwritten.
- The README claimed the language-specific rules lived in `CLAUDE.md` and
  told users to delete the sections they did not need, contradicting the
  copy-over update model.

### Migrating from 1.0.0

1.0.0 kept every rule in a single `CLAUDE.md`, alongside Communication Style,
Commit Style and Language Conventions in `project.md`. 2.0.0 moves all of it
into `.claude/rules/` and reduces `CLAUDE.md` to a nine-line entry point.

**Before copying anything**, diff your existing `CLAUDE.md` against
`git show v1.0.0:templates/CLAUDE.md`. Anything that differs is a rule you
customised, and the new `CLAUDE.md` will not contain it — the rules live in
`.claude/rules/` now. Carry those edits across to the matching rule file, or
to **Project Overrides** in `project.md` if they are specific to the project.

Then:

1. Replace `CLAUDE.md` with the new one, or append it if the file also holds
   project instructions of your own. Delete the duplicated `# CLAUDE.md`
   heading if you appended.
2. Remove the Communication Style, Commit Style and Language Conventions
   sections from `project.md`. They are now rule files. Move anything you had
   customised into the new **Project Overrides** section so it survives the
   next update.
3. Check the `paths:` frontmatter in `.claude/rules/2*.md` against this
   project's layout. The globs assume conventional directory names, and a
   pattern that matches nothing fails silently rather than erroring.
4. If you install the plugin from the marketplace, delete
   `.claude/skills/standards-ai/` — keeping both loads the skills twice.

Every later update is a straight file copy, or nothing at all if you symlink
`.claude/rules/`.

## [1.0.0] — 2026-08-17

Initial versioned release: `CLAUDE.md` and `project.md` templates, the
`standards-ai` plugin with the `about`, `review`, `fix-bug`, `preflight` and
`audit-violations` skills, and the `system-architect` and `code-reviewer`
agents.
