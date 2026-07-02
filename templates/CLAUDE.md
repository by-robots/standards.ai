# CLAUDE.md

@.claude/project.md

## Security — Read This First

- **Never** commit secrets, API keys, tokens, passwords, or credentials to the repository.
  Use environment variables or the framework's secrets management.
- **Never** run destructive operations (database drops, mass deletions, file removals) without explicit confirmation.
- **Never** disable or weaken authentication, authorisation, or encryption without explicit instruction.
- **Never** install dependencies or packages without asking first.
- **Never** expose internal paths, stack traces, or debug information in user-facing output.
- Always use parameterised queries. Never interpolate user input into SQL.
- Always validate and sanitise user input at the boundary.
- When writing access control logic, do not infer the required permission level from context. Ask explicitly what access should be granted before implementing it.
- Use the framework's built-in protections for mass assignment, CSRF, and input filtering. Never bypass them.
- If you're unsure whether something has security implications, **stop and ask**.

## Communication Preferences

- Give what is asked for — no more, no less.
- When viable approaches differ materially — in data model, public interface, or dependency footprint — present the trade-offs and ask which to pursue. Otherwise pick one and state your choice in one sentence.
- Be direct and honest. Do not affirm or compliment the user's statements before responding.
- If you're guessing, say so.
- Do not use emojis.
- Do not remove comments or existing code unless explicitly asked. If a change makes existing code redundant, flag it rather than deleting it silently.
- When referencing code, include the file path and line number, plus enough quoted context to survive the file changing.
- When suggesting changes, state the reasoning in one sentence. Do not elaborate unless asked.
- Ask clarifying questions when the intent or scope of a task is unclear rather than making assumptions.
- Before reporting a task complete, verify it — run the scoped tests or re-read the changed files — and state what you verified. If something could not be verified, say so plainly.

## Claude Code Specifics

- When a task requires changes across multiple files, list all affected
  files before starting.
- After making changes, run only the tests scoped to the affected files or modules and report the results. Do not run the full suite without asking.
- When an LSP is available, prefer it over Grep, Glob, and Read for code navigation tasks (find references, go-to-definition, symbol search).
- Use the Read, Edit, and Write tools to modify files. Do not use Python or other scripting languages for file edits. Avoid sed except for simple, single-operation substitutions.

## Coding Conventions

### General

- If code you write requires a comment to explain what it does, rewrite the code instead.
- Keep functions single-purpose. Split any function that does more than one distinct thing, or that cannot be summarised in a phrase without using "and".
- Follow existing patterns in the codebase.
- Change only what the task requires. Do not reformat, rename, or restructure code you are not otherwise touching.
- If the same error persists after two distinct fix attempts, stop and report what you tried. Do not attempt a third variation.
- Do not silently swallow exceptions or leave unhappy paths unhandled.
- Do not introduce abstraction unless it is used in more than one place.
- Prefer built-in and framework-provided operations over manual equivalents. If a single operation achieves what multiple steps do (e.g. upsert over separate create and update), use it.
- Identify edge cases before implementing. Raise them for discussion only when the correct handling is ambiguous; otherwise handle them and list them in your summary.

### Language & Framework

- Follow the established style guide for the project's language and
  framework. Do not deviate unless the project does so intentionally.
- Do not assume your knowledge of a library's API matches the version in use. When uncertain, look up the versioned documentation or read the installed dependency's source or type definitions rather than working from prior knowledge.
- Respect the project's linter and formatter configuration. Do not
  disable rules inline without explicit approval.
- Before writing any code that changes business logic, state which existing tests are affected and whether new tests are needed.
- New code for business logic must include tests. Use the project's existing
  test framework and follow its conventions.
- When touching business logic that has no tests, propose backfilling tests. If that is out of scope, say so explicitly.
- When asked to fix a bug, first write a failing test that reproduces it, then implement the fix. If the bug cannot be reproduced with a test — for example, in build scripts, infrastructure configuration, or database migrations — confirm with the user before proceeding without one.
- When identifying what tests are needed, address all relevant layers:
  unit tests for isolated logic, integration tests for component
  interactions, and endpoint-level tests for HTTP routes and CRUD operations.
- Keep business logic out of controllers, handlers, and other entry
  points. Extract it into service objects, plain classes, or modules.
- Scope data access through the current user or equivalent context rather than querying top-level models directly.
- Prefer explicit null/nil checks over implicit truthiness. Do not rely on falsy coercion when the value could be `0`, `""`, or an empty collection.

### Database

These rules apply only when the project has a database. Skip this section otherwise.

- Write reversible migrations. If a migration cannot be reversed, add a comment in the file explaining why.
- Add database-level constraints (not null, unique indexes, foreign keys)
  — do not rely solely on application-level validations.
- Do not write raw SQL in application code. Use the project's ORM or query builder. If raw SQL is unavoidable, document why in a comment.

### Performance

The database rules apply only when the project has a database. Skip those otherwise.

- Identify N+1 queries and resolve them using the ORM's eager-loading mechanisms. Do not rely on automatic or lazy loading.
- Avoid unnecessary database calls inside loops.
- Use batched iteration for large collections rather than loading
  everything into memory at once.

### Logging

- Never log tokens, passwords, API keys, or PII.
- Log messages should include the operation name, the relevant entity type and ID, and any values that distinguish one call from another.

### Accessibility

These rules apply only when the project produces user-facing HTML. Skip this section otherwise.

- Follow [WCAG](https://www.w3.org/WAI/standards-guidelines/wcag/) guidelines. At a minimum:
  - Use heading elements (`h1`–`h6`) in hierarchical order without skipping levels.
  - Do not use colour alone to convey information.

### Documentation

- Only add inline comments for non-obvious logic, decisions, or workarounds.
- Update READMEs and other documentation when changes affect setup, configuration, or usage.

### Dependency Management

- Before adding a dependency, check it with the ecosystem's audit tooling (`npm audit`, `bundle audit`, `pip-audit`) where available; otherwise say the check has not been done. Never assert from memory that a dependency has no known CVEs.
- Pin dependency versions in the project's dependency manifest.
- Avoid adding unnecessary dependencies — if the standard library or
  existing dependencies can do the job, prefer those.

## Git & Workflow

- Keep commits atomic — one logical change per commit.
- When the commit title alone does not explain why the change was made, add a body describing the reasoning.
- Do not amend or force-push shared branches without asking.
- Do not add co-author entries for AI tools in commit messages.

