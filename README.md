# standards.ai

Opinionated configuration templates for Claude Code. Drop them into a project
to get consistent, security-conscious AI assistance without repeating yourself.

The rules are my own preferences — go ahead and customise them to your needs.

The template follows the `CLAUDE.md` convention used by Claude Code, but the
format is broadly compatible with other AI coding tools that support
project-level instruction files. Other tools have not been tested.

> [!WARNING]
> Skills and agents read files and can consume a large number of tokens.
> `/standards-ai:review` scopes its checks by file type and asks before
> reading more than 20 files, but a wide target (`/standards-ai:review .`) or
> a run of the `code-reviewer` agent on a large branch can still be expensive.

## What's in the templates

Each template covers:

- **Security** — hard limits on secrets, destructive and outward-facing
  operations, injection risks, and treating fetched content as instructions.
- **Communication** — tone, language (UK English), and how the AI should
  propose changes.
- **Coding conventions** — general principles, testing, database,
  performance, logging, accessibility, and dependency management.
- **Language conventions** — style rules for Ruby, TypeScript/JavaScript,
  CSS, and mark-up.
- **Git workflow** — Conventional Commits, atomic changes, no committing
  unprompted or straight to the default branch, no AI co-author entries.
- **Project context** — a placeholder section you fill in per repository
  (stack, versions, architecture, deployment).

Sections that do not apply to your project — the database rules on a project
with no database, the Ruby rules on a TypeScript project — say so at the top
and are skipped. You do not need to delete them, and leaving them in place
keeps future updates a straight file copy.

## Usage

Copy the template files into your project:

```sh
cp templates/CLAUDE.md /path/to/your/project/CLAUDE.md
mkdir -p /path/to/your/project/.claude
cp -R templates/.claude/. /path/to/your/project/.claude/
```

The trailing `/.` matters. Most projects already have a `.claude` directory —
`cp -R templates/.claude <dest>/.claude/` would nest a second one inside it.

The templates are split into three files by how often you edit them:

| File | Contents | On update |
|------|----------|-----------|
| `CLAUDE.md` | Shared rules — security, communication, coding conventions, git | Overwritten |
| `.claude/conventions.md` | Shared style rules — UK English, Conventional Commits, Ruby, TypeScript, CSS, mark-up | Overwritten |
| `.claude/project.md` | **About This Project**, **Project Context**, **Project Overrides** | Never overwritten |

`CLAUDE.md` imports the other two at startup. Keeping everything you write by
hand in `project.md` means you can copy a newer `CLAUDE.md` and
`conventions.md` over the old ones without losing project-specific content.

Where a rule in `project.md` conflicts with the shared rules, the project rule
wins — use the **Project Overrides** section to record deliberate departures.

Run `/standards-ai:about` to populate the About and Project Context sections
automatically (see [Skills](#skills)), or fill them in manually.

### Versions

`CLAUDE.md` and `conventions.md` each carry a version marker on their first
line, so you can tell which release a project last received:

```sh
head -n 1 /path/to/your/project/CLAUDE.md
```

Compare it against [CHANGELOG.md](CHANGELOG.md) to see what has changed since,
then copy the newer files over. Updates are a straight file copy unless the
changelog says otherwise for that version.

`/standards-ai:sync` does this for you, and additionally warns about local
edits a copy would overwrite.

## Skills

Skills are reusable prompt templates invoked directly from Claude
Code's chat interface. They let you run common tasks against your project's
own rules without writing a prompt each time.

### Which review do I want?

Three things review code, and they do not overlap:

| | Checks | Runs tooling | Cost |
|---|---|---|---|
| `/standards-ai:review` | `CLAUDE.md` rule compliance | No | Low |
| `/standards-ai:preflight` | Linter, scoped tests, diff hygiene, docs, atomicity | Yes | Low |
| `code-reviewer` agent | Correctness bugs and security issues, plus rules | No | High |

`preflight` before every commit, `review` when you want a rule sweep, and the
agent for changes where a bug would be expensive — auth, payments, data
migrations, anything security-sensitive.

The skills (and agents) are packaged as a [skills-directory
plugin](https://code.claude.com/docs/en/plugins-reference#skills-directory-plugins)
named `standards-ai`. Nothing changes about installation — you still just copy
the files — but the skills load under a namespace (`/standards-ai:review`
rather than `/review`), which prevents them colliding with Claude Code's
built-in skills of the same name. Three things to be aware of:

- The plugin loads after you accept the workspace trust dialog.
- It loads from the `.claude/skills/` of the directory where Claude Code is
  launched. If you launch from a subdirectory, run `/reload-plugins`.
- Skills-directory plugins require a recent version of Claude Code.

### `/standards-ai:about`

Populates the **About This Project** and **Project Context** sections of your
rules file from project signals — README, package manifests, version files, and
deployment config. Run it once after copying the template into a new project.

Presents a draft for confirmation before writing anything. Pass a hint if the
project isn't self-describing from its files alone.

| Skill | Notes |
|-------|-------|
| `/standards-ai:about` | Infers from project files |
| `/standards-ai:about <hint>` | Uses hint to supplement inference |

### `/standards-ai:review`

Evaluates code against the rules defined in your project's `CLAUDE.md` and
`.claude/conventions.md`. Files are reviewed one at a time against only the
rule sections that can apply to that file type — a migration is not checked
against the mark-up rules — and violations are reported with the offending
code quoted alongside the rule being broken. Sections with no violations are
omitted, and the skill asks before reading more than 20 files.

Intended as a pre-commit check: run it against your staged changes before
pushing to catch rule violations early. It can also be pointed at a specific
file or directory for a more targeted review.

Violations recorded in `.claude/review-violations.md` are suppressed
automatically. If you dismiss a finding during a review session as acceptable,
the skill will offer to add it to that file.

| Skill | Scope |
|-------|-------|
| `/standards-ai:review` | Staged changes |
| `/standards-ai:review <path>` | Specified file or directory |

### `/standards-ai:fix-bug`

Fixes a bug using a test-first workflow: reproduce, write a failing test,
confirm it fails, implement the smallest fix, and run the scoped tests. Stops
and reports rather than thrashing if the fix does not land after two attempts.

The same workflow is required by the rules in `CLAUDE.md`; the skill makes it
an explicit step-by-step procedure, which smaller models follow more reliably
than a standalone rule.

| Skill | Notes |
|-------|-------|
| `/standards-ai:fix-bug <description>` | Description or location of the bug |

### `/standards-ai:preflight`

Runs a pre-commit checklist against staged changes: linter, scoped tests,
diff hygiene (debug statements, commented-out code, secrets), documentation
impact, and commit atomicity. Reports pass/fail per check and ends with a
ready-to-commit verdict.

| Skill | Scope |
|-------|-------|
| `/standards-ai:preflight` | Staged changes |

### `/standards-ai:commit`

Splits the current changes into atomic commits and writes Conventional Commit
messages for them. Presents the grouping and the full messages for approval
before staging anything, stages each group by path, and does not push.

Refuses to commit to the default branch without an explicit go-ahead, and
stops rather than guessing when a single file contains changes belonging to
two different commits.

| Skill | Scope |
|-------|-------|
| `/standards-ai:commit` | All uncommitted changes |
| `/standards-ai:commit <scope>` | Specified files or directories |

### `/standards-ai:audit-violations`

Audits `.claude/review-violations.md` for stale or imprecise entries. For each
entry, it checks whether the referenced file still exists, whether the context
identifier is still present, and whether the rule is still being violated. Entries
that no longer apply are proposed for removal; surviving entries are checked for
opportunities to improve their context or detail.

Presents all proposed changes for confirmation before writing anything.

| Skill | Notes |
|-------|-------|
| `/standards-ai:audit-violations` | Audits all entries in the violations register |

### `/standards-ai:sync`

Updates a project's template files from a local checkout of this repository.
Compares version markers, lists the changelog entries between the two
versions, and reports what each file copy would change.

Before copying, it checks each file against the version the project
originally received and quotes any local edit that overwriting would
discard — the one thing a straight `cp` loses silently. `project.md` and
`review-violations.md` are never touched.

| Skill | Notes |
|-------|-------|
| `/standards-ai:sync <path>` | Path to a standards.ai checkout |
| `/standards-ai:sync` | Asks for the path |

## Agents

Agents are specialised sub-agents that Claude Code can delegate work to. Unlike
skills, they run autonomously with their own context, tools, and persistent
memory — suited to tasks that require sustained focus on a single concern.

The agents ship inside the `standards-ai` plugin alongside the skills and are
discovered automatically when the plugin loads.

### `system-architect`

Handles architectural guidance, system design decisions, and trade-off
analysis. Invoked automatically when a request touches module structure,
component boundaries, data models, scalability, or integration patterns.

Follows a structured process: analyses the current codebase state, gathers
requirements, produces a design proposal (component diagram, responsibilities,
data models, interface definitions), and documents trade-offs with a
recommendation. Builds persistent memory of architectural decisions and
patterns discovered in the codebase across sessions.

The agent is intentionally language-neutral. Add language- or
framework-specific guidance to the agent file when adapting the template to
a target project.

### `code-reviewer`

Reviews a diff, branch, or pull request for correctness bugs, security
issues, and violations of the project's `CLAUDE.md` rules — in that order of
severity. Reports findings with the offending code quoted and a concrete
failure scenario; does not modify code.

Both agents inherit the session's model rather than pinning one, so they stay
current as models change. If you routinely work on a smaller model, add a
`model:` line to an agent's frontmatter to pin a stronger one for that agent.

Distinct from `/standards-ai:review`: the skill is a fast, mechanical
rule-compliance check suited to pre-commit use, while the agent applies
judgement to correctness and security and costs more to run. Both respect the
accepted violations recorded in `.claude/review-violations.md`. When a request
is ambiguous between the two, the agent asks rather than assuming — see
[Which review do I want?](#which-review-do-i-want).

## Repository structure

```
templates/
  CLAUDE.md                    # Shared rules template for Claude Code
  .claude/
    project.md                 # Project-specific context (About, Context, Overrides)
    conventions.md             # Shared style and language conventions
    review-violations.md       # Accepted violations suppressed by the review skill
    skills/
      standards-ai/            # Skills-directory plugin (loads as standards-ai)
        .claude-plugin/
          plugin.json          # Plugin manifest
        skills/
          about/
            SKILL.md           # Project setup skill
          review/
            SKILL.md           # Code review skill
          fix-bug/
            SKILL.md           # Test-first bug-fixing skill
          preflight/
            SKILL.md           # Pre-commit checklist skill
          commit/
            SKILL.md           # Atomic commit splitting and message writing
          audit-violations/
            SKILL.md           # Violations register maintenance skill
          sync/
            SKILL.md           # Template update skill
        agents/
          system-architect.md  # Sub-agent for architectural guidance and design
          code-reviewer.md     # Sub-agent for correctness and security review
.claude-plugin/
  marketplace.json             # Marketplace catalogue, for installing the plugin
CLAUDE.md                      # Rules for working on this repo itself (not a template)
CHANGELOG.md                   # Template version history and migration notes
```

## Contributing

Contributions are welcome. Open a PR.

## Licence

MIT with No-Resale Restriction — free to use, modify, and distribute,
but not to sell as a standalone product. See [LICENSE](LICENSE) for
details.
