---
name: review
description: Check code against the rules in .claude/rules/ and report violations. A mechanical rule-compliance pass — it runs no tooling and hunts no bugs. Use when asked to check staged changes or a path for rule violations. Not for pre-commit linting and tests (use the preflight skill) or for correctness and security judgement (use the code-reviewer agent).
---

Check code against the project's rules and report violations.

**Usage:**
- `/standards-ai:review` — checks staged changes only
- `/standards-ai:review <path>` — checks the file or directory at `<path>`

**Scope:** rule compliance only. Do not run the linter or the tests — that is
`/standards-ai:preflight`. Do not report bugs, security holes, or design
problems that no rule covers — that is the `code-reviewer` agent.

## Instructions

1. List the rule files: `.claude/rules/` (recursively), plus `CLAUDE.md`, any file it imports, and `.claude/project.md`. Read each rule file's frontmatter to get its `paths:` patterns. A rule file with no `paths:` applies to every file; one with `paths:` applies only to files matching those globs.
2. If `$ARGUMENTS` is provided, list the files at that path. Otherwise, run `git diff --name-only --cached` to get the list of staged files. Do not read the files under review yet.
3. If more than 20 files are in scope, report the count and ask which subset to review before reading any of them.
4. Match each file under review against the `paths:` patterns to build its rule set. Do not infer applicability from the rule file's title — a file matching no pattern of a scoped rule is not checked against it, and a rule with no `paths:` is always checked.
5. A rule whose text says it applies only under a condition the project does not meet — a database rule in a project with no database — is skipped for every file, whatever its paths match.
6. Rules in `.claude/project.md` override conflicting shared rules. Where they conflict, check against the project rule and do not report the shared one.
7. Read `.claude/review-violations.md` if it exists. When a violation is found, check whether it matches an entry by file path, context (method, class, or table/column name), and rule. If all three match, skip it.
8. For any migration file under review that appears to add a column or table without a database-level constraint (not null, unique index, foreign key), list all other migration files in the same directory, ordered by filename. If a migration with a later filename adds the missing constraint, do not flag the violation.
9. Review one file at a time. Read the file, check it against its rule set, and record any violations before moving to the next. Do not attempt to review all files in a single pass.
10. Report findings grouped by rule file (e.g. `00-security.md`, `03-general.md`).
11. For each finding, state the file and the offending code, and quote the rule being violated.
12. Omit rule files where no violations were found.
13. Do not suggest improvements beyond what the rules require.
14. End with a one-line summary: number of violations found and how many files were reviewed.
15. After presenting findings, if the user indicates a violation is acceptable or not applicable, offer to append an entry to `.claude/review-violations.md` with the file path, context identifier, rule, and reason.
