# standards.ai

Opinionated configuration templates for AI coding assistants. Drop them
into a project to get consistent, security-conscious AI assistance without
repeating yourself.

The rules are my own preferences — go ahead and customise them to your
needs.

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

The templates include language-specific sections for Ruby and
TypeScript/JavaScript. Keep the ones relevant to your stack and delete
the rest.

## Supported tools

| Tool | Template file | Destination |
|------|--------------|-------------|
| [Claude Code](https://docs.anthropic.com/en/docs/claude-code) | `templates/CLAUDE.md` | `CLAUDE.md` in project root |
| [Cursor](https://www.cursor.com/) | `templates/.cursorrules` | `.cursorrules` in project root |
| [Google Antigravity](https://antigravity.google/) | `templates/GEMINI.md` | `GEMINI.md` in project root |
| Any tool supporting AGENTS.md | `templates/AGENTS.md` | `AGENTS.md` in project root |

The tool-specific templates share the same core rules, each with a short
tool-specific section for behaviour that only applies to that tool.
`AGENTS.md` is a tool-agnostic alternative with no tool-specific section
— use it when your tool supports it or when you want a single file that
works across multiple tools.

## Usage

Copy the relevant file into the root of your project:

```sh
# Claude Code
cp templates/CLAUDE.md /path/to/your/project/CLAUDE.md

# Cursor
cp templates/.cursorrules /path/to/your/project/.cursorrules

# Google Antigravity
cp templates/GEMINI.md /path/to/your/project/GEMINI.md

# Tool-agnostic
cp templates/AGENTS.md /path/to/your/project/AGENTS.md
```

Then run `/about` to populate the **About This Project** and **Project
Context** sections automatically (see [Slash commands](#slash-commands)),
or fill them in manually.

## Slash commands

Slash commands are reusable prompt templates invoked directly from your AI
tool's chat interface. They let you run common tasks against your project's
own rules without writing a prompt each time.

### Setup

Copy the command files for your tool into your project alongside the rules
template. The destination path mirrors the source path under `templates/`:

```sh
# Claude Code
cp -r templates/.claude/commands /path/to/your/project/.claude/

# Cursor
cp -r templates/.cursor/commands /path/to/your/project/.cursor/

# Gemini CLI
cp -r templates/.gemini/commands /path/to/your/project/.gemini/
```

### `/about`

Populates the **About This Project** and **Project Context** sections of your
rules file from project signals — README, package manifests, version files, and
deployment config. Run it once after copying a template into a new project.

Presents a draft for confirmation before writing anything. Pass a hint if the
project isn't self-describing from its files alone.

| Tool | Command | Notes |
|------|---------|-------|
| Claude Code | `/about` | Infers from project files |
| Claude Code | `/about <hint>` | Uses hint to supplement inference |
| Cursor | `/about` | Infers from project files |
| Gemini CLI | `/about` | Infers from project files |
| Gemini CLI | `/about <hint>` | Uses hint to supplement inference |

**Limitations:** Cursor's custom slash commands do not reliably support
argument passing, so the Cursor version does not accept a hint argument.

### `/review`

Evaluates code against the rules defined in your project's rules file
(e.g. `CLAUDE.md`, `.cursorrules`). Each rule section is checked in turn and
violations are reported with the offending code quoted alongside the rule being
broken. Sections with no violations are omitted.

Intended as a pre-commit check: run it against your staged changes before
pushing to catch rule violations early. It can also be pointed at a specific
file or directory for a more targeted review.

| Tool | Command | Scope |
|------|---------|-------|
| Claude Code | `/review` | Staged changes |
| Claude Code | `/review <path>` | Specified file or directory |
| Cursor | `/review` | Staged changes only |
| Gemini CLI | `/review` | Staged changes |
| Gemini CLI | `/review <path>` | Specified file or directory |

**Limitations:** Cursor's custom slash commands do not reliably support
argument passing, so the Cursor version is limited to staged changes and
does not accept a path argument.

## Repository structure

```
templates/
  CLAUDE.md                    # Template for Claude Code
  .cursorrules                 # Template for Cursor
  GEMINI.md                    # Template for Google Antigravity
  AGENTS.md                    # Tool-agnostic template
  .claude/commands/
    about.md                   # Project setup command for Claude Code
    review.md                  # Code review command for Claude Code
  .cursor/commands/
    about.md                   # Project setup command for Cursor
    review.md                  # Code review command for Cursor
  .gemini/commands/
    about.toml                 # Project setup command for Gemini CLI
    review.toml                # Code review command for Gemini CLI
CLAUDE.md                      # Rules for working on this repo itself (not a template)
```

## Contributing

Contributions are welcome. If you have templates for additional AI tools
or improvements to existing ones, open a PR.

## Licence

MIT with No-Resale Restriction — free to use, modify, and distribute,
but not to sell as a standalone product. See [LICENSE](LICENSE) for
details.
