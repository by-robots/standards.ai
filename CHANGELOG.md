# Changelog

Notable changes to the templates. Each released version is stamped in the
first line of `templates/CLAUDE.md` and `templates/.claude/conventions.md`, so
you can tell which version a project last received.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and
versioning follows [Semantic Versioning](https://semver.org/):

- **Major** — a change requiring manual work in projects already using the
  template, such as moving content between files.
- **Minor** — new rules, skills, or agents.
- **Patch** — wording, clarifications, and fixes that do not change behaviour.

## [Unreleased]

### Added

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

### Fixed

- The documented install command nested a second `.claude` directory inside
  an existing one. Corrected to `cp -R templates/.claude/. <dest>/.claude/`.
- The README claimed the language-specific rules lived in `CLAUDE.md` and
  told users to delete the sections they did not need, contradicting the
  copy-over update model.

### Changed

- "Follow existing patterns in the codebase" and "Follow the established
  style guide" are replaced by one testable rule: read a comparable file
  first and match it, or say why you deviated. Neither original could be
  checked by reading the output.

- `code-reviewer` and `system-architect` no longer pin `model: opus`. A
  hardcoded model name ages badly in a template distributed across projects.
  Both inherit the session's model; a comment explains how to pin one.

- `review` scopes each file to the rule sections that can apply to its type
  rather than sweeping every rule against every file, and asks before
  reading more than 20 files. This addresses the token cost the README
  previously only warned about.

- Testing rules moved out of "Language & Framework" into their own
  **Testing** section. No wording changed; six of that section's rules were
  about tests and being buried there weakened them.

- The rule against removing code now exempts refactor, cleanup and removal
  tasks, where the blanket version guaranteed dead code accumulated and
  every session ended with a manual approval list.

- `review`, `preflight` and the `code-reviewer` agent now state their
  boundaries in the descriptions the model routes on, so an ambiguous
  "review my changes" no longer picks one at random. The agent asks which
  the user wants instead of defaulting to the expensive path.

## [1.1.0] — 2026-08-17

### Added

- `.claude/conventions.md` — Communication Style, Commit Style and Language
  Conventions moved here from `project.md` so updates can reach them.
- **Project Overrides** section in `project.md` for recording deliberate
  departures from the shared rules, and a precedence rule in `CLAUDE.md`
  stating that project rules win.
- Version markers in `CLAUDE.md` and `conventions.md`, and this changelog.

### Migrating from 1.0.0

Copying the new templates over the old ones leaves your existing
`project.md` untouched, which means Communication Style, Commit Style and
Language Conventions will be defined twice — once in your old `project.md`
and once in the new `conventions.md`.

The duplication is harmless where the two agree, but it wastes context and
`project.md` wins where they differ. After copying, remove those three
sections from `project.md`, moving anything you had customised into the new
**Project Overrides** section so it survives the next update. This is the
only manual step; every later update is a straight file copy.

## [1.0.0] — 2026-08-17

Initial versioned release: `CLAUDE.md` and `project.md` templates, the
`standards-ai` plugin with the `about`, `review`, `fix-bug`, `preflight` and
`audit-violations` skills, and the `system-architect` and `code-reviewer`
agents.
