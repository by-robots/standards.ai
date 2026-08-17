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
