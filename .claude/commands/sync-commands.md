Re-sync the commands in `templates/.claude/commands/` to the equivalent directories for all other AI tools.

## Background

`templates/.claude/commands/` is the source of truth for shared commands. Each command must exist in the format used by each other tool.

**Cursor** — `templates/.cursor/commands/<name>.md`

Plain markdown. Same structure and content as the Claude version. Where the Claude version references `$ARGUMENTS`, rephrase naturally (e.g. "if the user provided additional context in the command invocation").

**Gemini** — `templates/.gemini/commands/<name>.toml`

TOML with this structure:

```toml
description = "One-line description of what the command does and its usage."

prompt = """
<command content>
"""
```

Where the Claude version references `$ARGUMENTS`, replace with `{{args}}`.

Note: there is no command format for `AGENTS.md`. Only Cursor and Gemini need syncing.

## Instructions

1. List all files in `templates/.claude/commands/`.

2. For each command:
   a. Read the Claude version.
   b. Check whether an equivalent exists in `templates/.cursor/commands/` and `templates/.gemini/commands/`.
   c. If it exists, read it and compare the content against the Claude version.
   d. Note any files that are missing or have diverged from the Claude source.

3. Present all proposed additions and updates — including the full proposed content of each file — and ask for confirmation before writing anything.

4. On confirmation, write or update the files using the formats described above.

5. Report which files were created, which were updated, and which were already in sync.
