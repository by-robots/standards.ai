Re-sync the shared sections of `templates/CLAUDE.md` into all other tool template files.

## Background

`templates/CLAUDE.md` is the source of truth. All other templates must mirror its shared sections. Each template may also contain a tool-specific section (named `<Tool> Specifics`) that must not be overwritten.

## Shared sections

The following sections must be identical across all templates:

- Security — Read This First
- Communication Preferences
- Coding Conventions (all subsections)
- Git & Workflow
- Project Context

## Instructions

1. Read `templates/CLAUDE.md`.

2. Read each of the following files:
   - `templates/.cursorrules`
   - `templates/GEMINI.md`
   - `templates/AGENTS.md`

3. For each file, compare its shared sections against `templates/CLAUDE.md` line by line.

4. For each divergence found, report:
   - The file affected
   - The section containing the difference
   - The exact change needed to bring it into sync

5. Preserve each file's tool-specific section (any section named `<Tool> Specifics`) exactly as-is. Do not modify, move, or remove it.

6. Present all proposed changes and ask for confirmation before writing anything.

7. On confirmation, apply the changes. Report which files were updated and which were already in sync.
