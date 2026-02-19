Review staged changes against the rules defined in `.cursorrules`.

## Instructions

1. Read `.cursorrules` to identify all rules and conventions that apply to this project.
2. Run `git diff --name-only --cached` to get the list of staged files and read those.
3. For each rule in `.cursorrules`, check whether the code under review follows it.
4. Report findings grouped by `.cursorrules` section (e.g. Security, Coding Conventions).
5. For each finding, state the file and the offending code, and quote the rule being violated.
6. Omit sections where no violations were found.
7. Do not suggest improvements beyond what the rules require.
8. End with a one-line summary: number of violations found and how many files were reviewed.
