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
- When a task has multiple viable approaches, present them with trade-offs and ask which to pursue.
- Be direct and honest. Do not affirm or compliment the user's statements before responding.
- If you're guessing, say so.
- Use UK English (e.g. colour, organisation, authorise).
- Do not use emojis.
- Do not remove comments or existing code unless explicitly asked. If a change makes existing code redundant, flag it rather than deleting it silently.
- When referencing code, quote the relevant lines with enough surrounding context to identify their location. Do not cite line numbers alone.
- When suggesting changes, state the reasoning in one sentence. Do not elaborate unless asked.
- Ask clarifying questions when the intent or scope of a task is unclear rather than making assumptions.

## Claude Code Specifics

- When a task requires changes across multiple files, list all affected
  files before starting.
- After making changes, suggest the exact test commands to run, scoped to the affected files or modules. Do not run them.

## Coding Conventions

### General

- If code requires a comment to explain what it does, rewrite it.
- Keep functions single-purpose. Split any function that does more than one distinct thing, or that cannot be summarised in a phrase without using "and".
- Follow existing patterns in the codebase.
- Do not silently swallow exceptions or leave unhappy paths unhandled.
- Do not introduce abstraction unless it is used in more than one place.
- Prefer built-in and framework-provided operations over manual equivalents. If a single operation achieves what multiple steps do (e.g. upsert over separate create and update), use it.
- Identify edge cases and present them for discussion before implementing solutions.

### Language & Framework

- Follow the established style guide for the project's language and
  framework. Do not deviate unless the project does so intentionally.
- Do not assume your knowledge of a library's API matches the version in use. When uncertain, look up the versioned documentation rather than working from prior knowledge.
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

<!-- Keep the sections relevant to your stack. Delete the rest. -->

### Ruby

- Use `frozen_string_literal: true` in all Ruby files.
- Prefer `Hash#fetch` over `Hash#[]` when a missing key should raise.
- Prefer `present?` / `blank?` over nil checks when working with strings or collections in an ActiveSupport context.

### TypeScript / JavaScript

- Prefer `const` over `let`. Never use `var`.
- Do not disable `strict: true` in TypeScript projects where it is already configured.
- Prefer `Map.get` / `Map.has` over plain object property access for
  dynamic key lookups.

### CSS

- Follow the project's established CSS naming convention. If none exists, use [BEM](https://getbem.com/).
- Keep styles logically organised — one block per file or section.
  Do not mix unrelated blocks together.

### Mark-up

- Use elements for their intended purpose. Prefer `<button>` over `<div role="button">`,
  `<nav>` over `<div class="nav">`. Reserve `<div>` and `<span>` for cases where no
  semantic element fits.
- Use landmark elements (`<main>`, `<nav>`, `<header>`, `<footer>`, `<aside>`) to define
  page regions. Each page should have exactly one `<main>`.
- Validate mark-up against the HTML spec. Do not leave tags unclosed or elements
  improperly nested.
- Add `aria-label` or `aria-labelledby` when an element's purpose is not clear from its
  visible content or surrounding context alone.
- Use ARIA state attributes (`aria-expanded`, `aria-selected`, `aria-controls`, etc.) for
  interactive patterns with no native HTML equivalent. Do not use ARIA to override
  semantics that a native element already provides.
- Every `<img>` must have an `alt` attribute. Use `alt=""` for decorative images;
  describe the content for informative ones.

### Database

- Write reversible migrations. If a migration cannot be reversed, add a comment in the file explaining why.
- Add database-level constraints (not null, unique indexes, foreign keys)
  — do not rely solely on application-level validations.
- Do not write raw SQL in application code. Use the project's ORM or query builder. If raw SQL is unavoidable, document why in a comment.

### Performance

- Identify N+1 queries and resolve them using the ORM's eager-loading mechanisms. Do not rely on automatic or lazy loading.
- Avoid unnecessary database calls inside loops.
- Use batched iteration for large collections rather than loading
  everything into memory at once.

### Logging

- Never log tokens, passwords, API keys, or PII.
- Log messages should include the operation name, the relevant entity type and ID, and any values that distinguish one call from another.

### Accessibility

- Follow [WCAG](https://www.w3.org/WAI/standards-guidelines/wcag/) guidelines. At a minimum:
  - Use heading elements (`h1`–`h6`) in hierarchical order without skipping levels.
  - Do not use colour alone to convey information.

### Documentation

- Only add inline comments for non-obvious logic, decisions, or workarounds.
- Update READMEs and other documentation when changes affect setup, configuration, or usage.

### Dependency Management

- Before suggesting a dependency, check that it has had a release
  or commit within the last 12 months and has no known CVEs.
- Pin dependency versions in the project's dependency manifest.
- Avoid adding unnecessary dependencies — if the standard library or
  existing dependencies can do the job, prefer those.

## Git & Workflow

- Use [Conventional Commits](https://www.conventionalcommits.org/) for all commit messages.
- Keep commits atomic — one logical change per commit.
- When the commit title alone does not explain why the change was made, add a body describing the reasoning.
- Do not amend or force-push shared branches without asking.
- Do not add co-author entries for AI tools in commit messages.

