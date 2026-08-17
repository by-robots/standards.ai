<!-- standards-ai template v2.0.0 — https://github.com/by-robots/standards.ai -->

# CLAUDE.md

@.claude/project.md

Shared rules live in `.claude/rules/`. Files there without `paths:` frontmatter
load every session; the rest load when you touch a file they match.

Where a rule in `.claude/project.md` conflicts with one in `.claude/rules/`,
the project rule wins. Say which rule you applied and which it overrode.
