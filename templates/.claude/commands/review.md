Review code against the rules defined in CLAUDE.md.

**Usage:**
- `/review` — reviews staged changes only
- `/review <path>` — reviews the file or directory at `<path>`

## Instructions

1. Read `CLAUDE.md` to identify all rules and conventions that apply to this project.
2. If `$ARGUMENTS` is provided, read the files at that path. Otherwise, run `git diff --name-only --cached` to get the list of staged files and read those.
3. For any migration file under review that appears to add a column or table without a database-level constraint (not null, unique index, foreign key), list all other migration files in the same directory, ordered by filename. If a migration with a later filename adds the missing constraint, do not flag the violation.
4. For each rule in `CLAUDE.md`, check whether the code under review follows it.
5. Report findings grouped by `CLAUDE.md` section (e.g. Security, Coding Conventions).
6. For each finding, state the file and the offending code, and quote the rule being violated.
7. Omit sections where no violations were found.
8. Do not suggest improvements beyond what the rules require.
9. End with a one-line summary: number of violations found and how many files were reviewed.
