# CLAUDE.md

## About This Repository

This repository contains template rule files for AI coding assistants. The
templates are copied into other projects to define how AI tools should behave
when building software. The templates themselves are the product — not
application code.

The primary template targets Claude Code (`CLAUDE.md`). Rules that apply
regardless of tool are then translated into formats for other assistants:
Cursor (`.cursorrules`), Gemini (`GEMINI.md`), and Codex/Copilot (`AGENTS.md`).
Each tool also has a commands directory with slash commands translated into
its native format. When working here, assume Claude is the source of truth
and other formats are derived from it.

## Working on This Repository

### What We Are Building

Effective, opinionated rule sets that produce consistent, high-quality AI
assistance across projects. Every rule should earn its place: vague or
unenforceable guidance is worse than no guidance.

### Guiding Principles

- **Rules must be actionable.** Each rule should describe a specific behaviour
  the AI can follow or avoid. "Write good code" is not a rule; "Keep methods
  under 15 lines" is.
- **Rules must be testable.** If you cannot tell whether a rule was followed by
  reading the AI's output, the rule needs rewriting.
- **Brevity matters.** AI context windows are finite. Every word in a rules
  file competes with the user's actual work for attention. Be concise.
- **Order matters.** Place the most critical rules (security, communication
  preferences) first. AI tools weight earlier instructions more heavily.
- **Consistency across templates.** When a rule applies regardless of AI tool,
  keep the wording identical across all template files. Divergence should only
  exist where tool-specific behaviour requires it.

### How to Help

When asked to add, remove, or modify rules:

1. **Understand the intent.** Ask what behaviour the rule is meant to produce
   or prevent. A rule is a response to a real problem — understand the problem
   first.
2. **Check for conflicts.** Read the existing rules before adding new ones.
   New rules must not contradict existing ones without explicitly superseding
   them.
3. **Propagate changes.** When a rule change applies across tools, update all
   relevant template files. Use `/sync-rules` to propagate rule changes and
   `/sync-commands` to propagate command changes. Flag any files not updated.
4. **Preserve structure.** Follow the existing section hierarchy. Do not
   reorganise sections without being asked.
5. **Explain trade-offs.** If a proposed rule has downsides (e.g. reduces
   flexibility, increases verbosity), say so.

### What Not to Do

- Do not add rules that are obvious or already implied by the AI's default
  behaviour. The goal is to override defaults or enforce project-specific
  standards, not to restate common sense.
- Do not add tool-specific syntax or features to the wrong template file.
  Each template targets a specific tool.
- Do not pad rules with filler or qualifications. If a rule needs three
  paragraphs to explain, it is probably too complex to enforce.
- Do not invent rules based on general best practices alone. Rules should
  reflect the repository owner's actual preferences and workflow.

## Communication Preferences

- Use UK English (e.g. colour, organisation, authorise).
- Be direct. No flattery.
- If you are guessing, say so.
- Do not use emojis.
- When suggesting rule changes, show the exact wording you propose — not
  a summary of what it would say.

## Repository Structure

```
templates/
  CLAUDE.md            # Template rules for Claude Code
  .cursorrules         # Template rules for Cursor
  GEMINI.md            # Template rules for Gemini
  AGENTS.md            # Template rules for Codex/Copilot
  .claude/commands/    # Slash commands for Claude Code
  .cursor/commands/    # Slash commands for Cursor
  .gemini/commands/    # Slash commands for Gemini CLI
.claude/commands/      # Project-level commands for this repository
```

Templates are copied into target projects and adapted per project. The
`Project Context` section at the bottom of each template is intentionally
left as a placeholder for per-project customisation.

The `templates/.claude/commands/` directory is the source of truth for shared
commands. Cursor and Gemini equivalents are derived from it using `/sync-commands`.

## Git & Workflow

- Use [Conventional Commits](https://www.conventionalcommits.org/).
- Keep commits atomic — one logical change per commit.
- When a change touches multiple template files, include all affected files
  in the same commit.
- Do not add co-author entries for AI tools in commit messages.
