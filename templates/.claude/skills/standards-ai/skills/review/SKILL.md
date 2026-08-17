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

1. Read `CLAUDE.md` to identify all rules and conventions that apply to this project.
2. If `$ARGUMENTS` is provided, read the files at that path. Otherwise, run `git diff --name-only --cached` to get the list of staged files and read those.
3. Read `.claude/review-violations.md` if it exists. When a violation is found, check whether it matches an entry by file path, context (method, class, or table/column name), and rule. If all three match, skip it.
4. For any migration file under review that appears to add a column or table without a database-level constraint (not null, unique index, foreign key), list all other migration files in the same directory, ordered by filename. If a migration with a later filename adds the missing constraint, do not flag the violation.
5. Review one file at a time. For each file, check every rule in `CLAUDE.md` and record any violations before moving to the next file. Do not attempt to review all files in a single pass.
6. Report findings grouped by `CLAUDE.md` section (e.g. Security, Coding Conventions).
7. For each finding, state the file and the offending code, and quote the rule being violated.
8. Omit sections where no violations were found.
9. Do not suggest improvements beyond what the rules require.
10. End with a one-line summary: number of violations found and how many files were reviewed.
11. After presenting findings, if the user indicates a violation is acceptable or not applicable, offer to append an entry to `.claude/review-violations.md` with the file path, context identifier, rule, and reason.
