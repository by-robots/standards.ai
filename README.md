# standards.ai

Opinionated configuration for Claude Code — a rules template plus a plugin of
skills and agents. Install the plugin once, add the rules to a project, and get
consistent, security-conscious AI assistance without repeating yourself.

The rules are my own preferences — go ahead and customise them to your needs.

The template follows the `CLAUDE.md` convention used by Claude Code, but the
format is broadly compatible with other AI coding tools that support
project-level instruction files. Other tools have not been tested.

> [!WARNING]
> Skills and agents read files and can consume a large number of tokens.
> `/standards-ai:review` scopes its checks by file type and asks before
> reading more than 20 files, but a wide target (`/standards-ai:review .`) or
> a run of the `code-reviewer` agent on a large branch can still be expensive.

## What's in the rules

The rules template covers:

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

standards.ai has two halves, and they install differently:

| Half | What it is | How it gets into a project |
|------|------------|----------------------------|
| **Plugin** | The skills and agents | Installed once per machine. Never copied. |
| **Rules** | `CLAUDE.md` and `.claude/conventions.md` | Per project, by copy or by import |

Clone this repository somewhere permanent first — the examples below assume
`~/Code/standards.ai`:

```sh
git clone git@github.com:by-robots/standards.ai.git ~/Code/standards.ai
```

### 1. Install the plugin

This repository is a plugin marketplace. Install the skills and agents once
and they are available in every project, with nothing copied anywhere. From
any Claude Code session:

```
/plugin marketplace add ~/Code/standards.ai
/plugin install standards-ai@standards-ai
```

If the install summary says `Run /reload-plugins to activate.`, run that. The
same works from the shell with `claude plugin marketplace add` and
`claude plugin install`.

Marketplace state is stored once per user in
`~/.claude/plugins/known_marketplaces.json`, so this survives across projects
and worktrees. Skills load namespaced, as `/standards-ai:review` rather than
`/review`, so they cannot collide with Claude Code's built-in skills.

### 2. Add the rules to a project

The rules are project files, so they need to reach each project. Pick one of
two approaches — copy for repositories you share, import for your own.

#### Option A: copy (default)

Real files, committed to the project, working for anyone who clones it:

```sh
cp ~/Code/standards.ai/templates/CLAUDE.md /path/to/your/project/CLAUDE.md
mkdir -p /path/to/your/project/.claude
cp -R ~/Code/standards.ai/templates/.claude/. /path/to/your/project/.claude/
```

The trailing `/.` matters. Most projects already have a `.claude` directory —
`cp -R templates/.claude <dest>/.claude/` would nest a second one inside it.

If you installed the plugin in step 1, delete the copied
`.claude/skills/standards-ai/` afterwards; keeping both loads the skills
twice.

#### Option B: import (auto-updating)

Instead of copying the two shared files, point at them. Put this at the top of
the project's `CLAUDE.md`, above its own content:

```
@~/Code/standards.ai/templates/CLAUDE.md
@~/Code/standards.ai/templates/.claude/conventions.md
```

Then `git pull` in `~/Code/standards.ai` updates every project at once. You
still create `.claude/project.md` per project — that is the file holding
content specific to the project.

Three things to know before choosing this:

- **Anchor to `~`, not `../`.** Relative imports resolve against the file
  containing them, so `../standards.ai` means something different for every
  project depending on how deeply it is nested. `~/` is depth-independent.
- **It prompts once per project.** An import resolving outside the working
  directory triggers an approval dialog listing the files. Accept it and the
  imports load from then on. Decline it and they stay disabled permanently
  for that project, without asking again.
- **The path only resolves on your machine.** For a repository anyone else
  clones — a teammate, CI, Claude Code on the web — the import is broken. In
  a shared project put the two lines in a gitignored `CLAUDE.local.md`
  instead of `CLAUDE.md`.

The trade is deliberate. Copying makes each update a visible, diffable event
you choose to accept; importing keeps every project current at the cost of
rules changing under you the moment you commit to standards.ai. Mixing the
two is fine — import in your own projects, copy in the shared ones.

### 3. Fill in the project file

Whichever option you chose, run `/standards-ai:about` to populate the
**About This Project** and **Project Context** sections of
`.claude/project.md`, or fill them in by hand.

### Updating

**The plugin.** Pull, refresh the marketplace, update the plugin:

```sh
git -C ~/Code/standards.ai pull
```

```
/plugin marketplace update standards-ai
/plugin update standards-ai@standards-ai
```

A restart is required for the update to take effect. Updates are delivered
only when the `version` in the plugin manifest changes, so pulling a commit
that does not bump the version changes nothing.

**Imported rules (Option B).** Nothing to do. The `git pull` above is the
update; the next session picks it up.

**Copied rules (Option A).** Pull, then run `/standards-ai:sync` in the target
project:

```
/standards-ai:sync ~/Code/standards.ai
```

It compares version markers, lists the changelog entries between the two
versions, and quotes any local edit the copy would overwrite before it copies
anything. To do it by hand, re-run the `cp` commands from Option A and check
[CHANGELOG.md](CHANGELOG.md) for a migration note first.

### The three rule files

The rules are split into three files by how often you edit them:

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

### Versions

`CLAUDE.md` and `conventions.md` each carry a version marker on their first
line, so you can tell which release a project last received:

```sh
head -n 1 /path/to/your/project/CLAUDE.md
```

Compare it against [CHANGELOG.md](CHANGELOG.md) to see what has changed since.
Projects using the import (Option B) always read the current version, so the
marker only matters for copied rules.

The plugin is versioned separately in its own manifest. `/plugin list` shows
the installed version.

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

The skills and agents are packaged as a plugin named `standards-ai`, installed
from this repository as a marketplace — see [Install the
plugin](#1-install-the-plugin). Skills load under a namespace
(`/standards-ai:review` rather than `/review`), so they cannot collide with
Claude Code's built-in skills of the same name.

The plugin can also be loaded without installing it, by copying the tree to a
project's `.claude/skills/` as a [skills-directory
plugin](https://code.claude.com/docs/en/plugins-reference#skills-directory-plugins).
That is the older approach and it needs no marketplace, but it copies the
skills into every project and loads only from the directory Claude Code is
launched in — run `/reload-plugins` if you launch from a subdirectory. Use one
approach or the other; both at once loads the skills twice.

### `/standards-ai:about`

Populates the **About This Project** and **Project Context** sections of your
rules file from project signals — README, package manifests, version files, and
deployment config. Run it once after adding the rules to a new project.

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

Updates a project's copied rule files from a local checkout of this
repository. Compares version markers, lists the changelog entries between the
two versions, and reports what each file copy would change.

Only relevant to projects that copied the rules (Option A). Projects using the
import always read the current version, and the plugin updates through
`/plugin update`.

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
