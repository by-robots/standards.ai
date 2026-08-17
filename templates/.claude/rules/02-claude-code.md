# Claude Code Specifics

- When a task requires changes across multiple files, list all affected files before starting.
- After making changes, run only the tests scoped to the affected files or modules and report the results. Do not run the full suite without asking.
- When an LSP is available, prefer it over Grep, Glob, and Read for code navigation tasks (find references, go-to-definition, symbol search).
- Use the Read, Edit, and Write tools to modify files. Do not use Python or other scripting languages for file edits. Avoid sed except for simple, single-operation substitutions.
- Do not create files the task did not ask for — no summary documents, no migration notes, no scratch files in the repository. Report in your response instead.
- Do not start dev servers, watchers, or other long-running processes without asking.
