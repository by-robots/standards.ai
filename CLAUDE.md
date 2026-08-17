# CLAUDE.md

## About This Repository

This repository contains a template rule file for Claude Code. The template
is copied into other projects to define how Claude should behave when building
software. The template itself is the product — not application code.

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

### How to Help

When asked to add, remove, or modify rules:

1. **Understand the intent.** Ask what behaviour the rule is meant to produce
   or prevent. A rule is a response to a real problem — understand the problem
   first.
2. **Check for conflicts.** Read the existing rules before adding new ones.
   New rules must not contradict existing ones without explicitly superseding
   them.
3. **Preserve structure.** Follow the existing section hierarchy. Do not
   reorganise sections without being asked.
4. **Explain trade-offs.** If a proposed rule has downsides (e.g. reduces
   flexibility, increases verbosity), say so.

### Keeping the README in Sync

When making changes that affect repository structure, features, or usage,
check whether `README.md` needs updating before closing the task.

### Versioning the Templates

`templates/CLAUDE.md` and `templates/.claude/conventions.md` carry a version
marker on their first line. When changing either file, add an entry under
`## [Unreleased]` in `CHANGELOG.md`. When cutting a release, bump the markers,
`plugin.json`, and the changelog heading together in one commit.

Bump major only when a change cannot be applied by copying files over the old
ones. Any such release must include a "Migrating from" note in the changelog.

### What Not to Do

- Do not add rules that are obvious or already implied by the AI's default
  behaviour. The goal is to override defaults or enforce project-specific
  standards, not to restate common sense.
- Do not pad rules with filler or qualifications. If a rule needs three
  paragraphs to explain, it is probably too complex to enforce.
- Do not invent rules based on general best practices alone. Rules should
  reflect the repository owner's actual preferences and workflow.

## Communication Preferences

- Use UK English (e.g. colour, organisation, authorise).
- Be direct. No flattery.
- When the user states an opinion or proposes an approach, identify the strongest counter-argument or failure mode before agreeing or implementing it. If no substantive objection exists, say the approach holds — do not manufacture one.
- If you are guessing, say so.
- Do not use emojis.
- When suggesting rule changes, show the exact wording you propose — not
  a summary of what it would say.

## Repository Structure

```
templates/
  CLAUDE.md            # Shared rules template for Claude Code
  .claude/
    project.md         # Per-project content (About, Context, Overrides)
    conventions.md     # Shared style and language conventions
    skills/
      standards-ai/    # Skills-directory plugin bundling the skills and agents
.claude-plugin/
  marketplace.json     # Marketplace catalogue pointing at the plugin above
.claude/skills/        # Project-level skills for this repository
```

This repository is itself a plugin marketplace. `marketplace.json` lists the
plugin with a path relative to the repository root — absolute paths fail
validation. It carries no `version`; the version comes from the plugin's own
`plugin.json`, so there is one place to bump. Run `claude plugin validate .`
after changing either manifest.

The template is copied into target projects and adapted per project.
`CLAUDE.md` and `.claude/conventions.md` are overwritten on update;
`.claude/project.md` is not, and holds everything the user writes by hand.
When adding a rule, put it in `conventions.md` if it is language- or
style-specific, and in `CLAUDE.md` otherwise. Never add shared rules to
`project.md` — updates cannot reach them there.

## Git & Workflow

- Use [Conventional Commits](https://www.conventionalcommits.org/).
- Keep commits atomic — one logical change per commit.
- When a change touches multiple files, include all affected files in the
  same commit.
- Do not add co-author entries for AI tools in commit messages.
