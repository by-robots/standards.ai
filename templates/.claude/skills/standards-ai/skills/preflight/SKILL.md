---
name: preflight
description: Run the project's linter and scoped tests against staged changes, then check the diff for debug statements, secrets, documentation impact, and commit atomicity. Use immediately before committing. Does not sweep the project's rules (use the review skill) or hunt for bugs (use the code-reviewer agent).
---

Run a pre-commit checklist against the staged changes.

**Usage:**
- `/standards-ai:preflight` — checks staged changes; stages and commits nothing itself

**Scope:** tooling and diff hygiene only. Do not sweep the rules in
`.claude/rules/` — that is `/standards-ai:review`. The hygiene checks below are
fixed; run them as written rather than expanding them into a rule audit.

## Instructions

Run every check in order. Do not stop at the first failure — complete all
checks and report the findings together.

1. Run `git diff --name-only --cached` to list the staged files. If nothing is staged, report that and stop.
2. Run the project's linter or formatter in check mode on the staged files, if one is configured. Report violations; do not auto-fix unless asked.
3. Run the tests scoped to the staged files or their modules. Do not run the full suite without asking.
4. Read the staged diff (`git diff --cached`) and check for:
   - debug statements (`console.log`, `print`, `binding.pry`, `debugger`, `dd`)
   - commented-out code
   - TODO or FIXME comments added without an issue reference
   - secrets, keys, or tokens
5. Check whether the staged changes affect setup, configuration, or usage described in the README or other documentation. Flag any documentation that needs updating.
6. Check that the staged changes form one logical change. If unrelated changes are mixed together, suggest how to split them.
7. Report the results as a checklist — one line per check, pass or fail, with findings underneath. End with a one-line verdict: ready to commit, or not, and why.
8. If the verdict is "ready to commit", add one line suggesting `/standards-ai:review` for a rule-compliance pass if the change has not had one.
