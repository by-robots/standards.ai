# CLAUDE.md

## About This Repository

This is a template repository for AI coding assistant rules. It contains
configuration files (`CLAUDE.md`, `.cursorrules`, etc.) that are copied into
other projects to define how AI tools should behave. The templates themselves
are the product — not application code.

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
  keep the wording identical across `CLAUDE.md`, `.cursorrules`, and any
  other format. Divergence should only exist where tool-specific behaviour
  requires it.

### How to Help

When asked to add, remove, or modify rules:

1. **Understand the intent.** Ask what behaviour the rule is meant to produce
   or prevent. A rule is a response to a real problem — understand the problem
   first.
2. **Check for conflicts.** Read the existing rules before adding new ones.
   New rules must not contradict existing ones without explicitly superseding
   them.
3. **Propagate changes.** When a rule change applies across tools, update all
   relevant template files. Flag any files that were not updated.
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
  CLAUDE.md        # Template rules for Claude Code
  .cursorrules     # Template rules for Cursor
```

Templates are copied into target projects and adapted per project. The
`Project Context` section at the bottom of each template is intentionally
left as a placeholder for per-project customisation.

## Git & Workflow

- Use [Conventional Commits](https://www.conventionalcommits.org/).
- Keep commits atomic — one logical change per commit.
- When a change touches multiple template files, include all affected files
  in the same commit.
