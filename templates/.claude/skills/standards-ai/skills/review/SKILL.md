---
name: review
description: Check code against the rules in CLAUDE.md and report violations. A mechanical rule-compliance pass — it runs no tooling and hunts no bugs. Use when asked to check staged changes or a path for rule violations. Not for pre-commit linting and tests (use the preflight skill) or for correctness and security judgement (use the code-reviewer agent).
---

Check code against the rules defined in CLAUDE.md and its imports.

**Usage:**
- `/standards-ai:review` — checks staged changes only
- `/standards-ai:review <path>` — checks the file or directory at `<path>`

**Scope:** rule compliance only. Do not run the linter or the tests — that is
`/standards-ai:preflight`. Do not report bugs, security holes, or design
problems that no rule covers — that is the `code-reviewer` agent.

## Instructions

1. Read `CLAUDE.md` and the files it imports to identify the rules that apply to this project.
2. If `$ARGUMENTS` is provided, list the files at that path. Otherwise, run `git diff --name-only --cached` to get the list of staged files. Do not read the files yet.
3. Build the rule set for each file from its type before reading it. Check only the sections that can apply:
   - Always: Security, Coding Conventions — General, Documentation, Logging.
   - Source files: Language & Framework, Testing, Performance, and the matching language section in `.claude/conventions.md`.
   - Migrations and schema files: Database, Performance.
   - Templates, components, and other files producing HTML: Accessibility, and the CSS and Mark-up sections in `.claude/conventions.md`.
   - Manifests and lockfiles: Dependency Management.

   A section whose preamble says it applies only under a condition the project does not meet — a database section with no database, a Ruby section in a TypeScript project — is skipped for every file.
4. Read `.claude/review-violations.md` if it exists. When a violation is found, check whether it matches an entry by file path, context (method, class, or table/column name), and rule. If all three match, skip it.
5. For any migration file under review that appears to add a column or table without a database-level constraint (not null, unique index, foreign key), list all other migration files in the same directory, ordered by filename. If a migration with a later filename adds the missing constraint, do not flag the violation.
6. Review one file at a time. Read the file, check it against its rule set from step 3, and record any violations before moving to the next. Do not attempt to review all files in a single pass.
7. If more than 20 files are in scope, report the count and ask which subset to review before reading any of them.
8. Report findings grouped by rule section (e.g. Security, Coding Conventions).
9. For each finding, state the file and the offending code, and quote the rule being violated.
10. Omit sections where no violations were found.
11. Do not suggest improvements beyond what the rules require.
12. End with a one-line summary: number of violations found and how many files were reviewed.
13. After presenting findings, if the user indicates a violation is acceptable or not applicable, offer to append an entry to `.claude/review-violations.md` with the file path, context identifier, rule, and reason.
