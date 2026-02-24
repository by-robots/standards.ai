# standards.ai

Opinionated configuration templates for Claude Code. Drop them into a project
to get consistent, security-conscious AI assistance without repeating yourself.

The rules are my own preferences — go ahead and customise them to your needs.

> [!WARNING]
> Use caution when running commands. Some will use a large amount of tokens
> if used without restriction (e.g. `/review .`) and could easily exhaust your
> usage limits or incur costs.

## What's in the templates

Each template covers:

- **Security** — hard limits on secrets, destructive operations, and
  injection risks.
- **Communication** — tone, language (UK English), and how the AI should
  propose changes.
- **Coding conventions** — general principles plus language-specific rules
  for Ruby, TypeScript/JavaScript, CSS, and database migrations.
- **Git workflow** — Conventional Commits, atomic changes, no AI
  co-author entries.
- **Project context** — a placeholder section you fill in per repository
  (stack, versions, architecture, deployment).

The template includes language-specific sections for Ruby and
TypeScript/JavaScript. Keep the ones relevant to your stack and delete
the rest.

## Usage

Copy the template files into your project:

```sh
cp templates/CLAUDE.md /path/to/your/project/CLAUDE.md
cp -r templates/.claude /path/to/your/project/.claude/
```

`CLAUDE.md` contains the shared rules and imports `.claude/project.md` at
startup. The project file holds the two sections specific to your project —
**About This Project** and **Project Context** — and is kept separate so you
can update the shared rules without overwriting your project-specific content.

Run `/about` to populate those sections automatically (see
[Slash commands](#slash-commands)), or fill them in manually.

## Slash commands

Slash commands are reusable prompt templates invoked directly from Claude
Code's chat interface. They let you run common tasks against your project's
own rules without writing a prompt each time.

### `/about`

Populates the **About This Project** and **Project Context** sections of your
rules file from project signals — README, package manifests, version files, and
deployment config. Run it once after copying the template into a new project.

Presents a draft for confirmation before writing anything. Pass a hint if the
project isn't self-describing from its files alone.

| Command | Notes |
|---------|-------|
| `/about` | Infers from project files |
| `/about <hint>` | Uses hint to supplement inference |

### `/review`

Evaluates code against the rules defined in your project's `CLAUDE.md`. Each
rule section is checked in turn and violations are reported with the offending
code quoted alongside the rule being broken. Sections with no violations are
omitted.

Intended as a pre-commit check: run it against your staged changes before
pushing to catch rule violations early. It can also be pointed at a specific
file or directory for a more targeted review.

Violations recorded in `.claude/review-violations.md` are suppressed
automatically. If you dismiss a finding during a review session as acceptable,
the command will offer to add it to that file.

| Command | Scope |
|---------|-------|
| `/review` | Staged changes |
| `/review <path>` | Specified file or directory |

### `/audit-violations`

Audits `.claude/review-violations.md` for stale or imprecise entries. For each
entry, it checks whether the referenced file still exists, whether the context
identifier is still present, and whether the rule is still being violated. Entries
that no longer apply are proposed for removal; surviving entries are checked for
opportunities to improve their context or detail.

Presents all proposed changes for confirmation before writing anything.

| Command | Notes |
|---------|-------|
| `/audit-violations` | Audits all entries in the violations register |

## Repository structure

```
templates/
  CLAUDE.md                    # Shared rules template for Claude Code
  .claude/
    project.md                 # Project-specific context (About, Stack, etc.)
    review-violations.md       # Accepted violations suppressed by /review
    commands/
      about.md                 # Project setup command
      review.md                # Code review command
      audit-violations.md      # Violations register maintenance command
CLAUDE.md                      # Rules for working on this repo itself (not a template)
```

## Contributing

Contributions are welcome. Open a PR.

## Licence

MIT with No-Resale Restriction — free to use, modify, and distribute,
but not to sell as a standalone product. See [LICENSE](LICENSE) for
details.
