# standards.ai

Opinionated configuration templates for AI coding assistants. Drop them
into a project to get consistent, security-conscious AI assistance without
repeating yourself.

The rules are my own preferences — go ahead and customise them to your
needs.

## What's in the templates

Each template covers:

- **Security** — hard limits on secrets, destructive operations, and
  injection risks.
- **Communication** — tone, language (UK English), and how the AI should
  propose changes.
- **Coding conventions** — general principles plus language-specific rules
  for Ruby/Rails, JavaScript, SCSS, and database migrations.
- **Git workflow** — Conventional Commits, atomic changes, no AI
  co-author entries.
- **Project context** — a placeholder section you fill in per repository
  (stack, versions, architecture, deployment).

The templates are opinionated toward a Ruby on Rails stack. If your
project uses a different stack, strip or replace the language-specific
sections.

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

Then edit the **Project Context** section at the bottom to describe your
stack, versions, key dependencies, and architecture.

## Repository structure

```
templates/
  CLAUDE.md        # Template for Claude Code
  .cursorrules     # Template for Cursor
  GEMINI.md        # Template for Google Antigravity
  AGENTS.md        # Tool-agnostic template
CLAUDE.md          # Rules for working on this repo itself (not a template)
```

## Contributing

Contributions are welcome. If you have templates for additional AI tools
or improvements to existing ones, open a PR.

## Licence

MIT with No-Resale Restriction — free to use, modify, and distribute,
but not to sell as a standalone product. See [LICENSE](LICENSE) for
details.
