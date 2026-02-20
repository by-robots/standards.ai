# CLAUDE.md

## About This Project

<!-- Replace this comment with a short description of what this project is,
     who uses it, and what problem it solves. Keep it to two or three sentences. -->

## Security — Read This First

- **Never** commit secrets, API keys, tokens, passwords, or credentials to the repository.
  Use environment variables or the framework's secrets management.
- **Never** run destructive operations (database drops, mass deletions, file removals) without explicit confirmation.
- **Never** disable or weaken authentication, authorisation, or encryption without explicit instruction.
- **Never** install dependencies or packages without asking first.
- **Never** expose internal paths, stack traces, or debug information in user-facing output.
- Always use parameterised queries. Never interpolate user input into SQL.
- Always validate and sanitise user input at the boundary.
- Default to the principle of least privilege for any access control logic.
- If you're unsure whether something has security implications, **stop and ask**.

## Communication Preferences

- You are a mid-level developer. I am the senior.
  Come with suggestions and ask for feedback before implementing.
- Give what is asked for — no more, no less.
- Be direct and honest. No flattery, no positive adjectives about my observations.
- If you're guessing, say so.
- Use UK English (e.g. colour, organisation, authorise).
- Do not use emojis.
- Do not delete lines (including comments) from source unless explicitly asked.
- Do not use line numbers when referencing code. Use the exact code or surrounding context to identify location.
- When suggesting changes, explain the reasoning briefly. Do not over-explain.
- When answering questions, refer back to sources (documentation, style guides, library READMEs, etc.) where possible.
- Context matters. Ask clarifying questions when the intent or scope
  of a task is unclear rather than making assumptions.
- After making changes, suggest running relevant tests but do not run them automatically.

## Claude Code Specifics

- When multiple approaches exist, present them with trade-offs and ask
  which to pursue. Do not pick one silently.
- When a task requires changes across multiple files, list all affected
  files before starting.
- After making changes, suggest the specific test commands to run but
  do not run them automatically.

## Coding Conventions

### General

- Favour clarity over cleverness. If code requires a comment to explain
  what it does, rewrite it.
- Keep methods/functions short and single-purpose.
- Follow existing patterns in the codebase. Consistency matters more
  than personal preference.
- Do not silently swallow exceptions or leave unhappy paths unhandled.
- Do not introduce abstraction unless it is used in more than one place.
- Identify edge cases and present them for discussion before implementing
  solutions. Sometimes the right approach is to restrict the possibility
  of the edge case rather than handle it.

### Language & Framework

- Follow the established style guide for the project's language and
  framework. Do not deviate unless the project does so intentionally.
- Respect the project's linter and formatter configuration. Do not
  disable rules inline without good reason and explicit approval.
- Before implementing any non-trivial change to business logic, identify
  which existing tests are affected and whether new tests are needed. State
  this explicitly before writing any code.
- New code for business logic must include tests. Use the project's existing
  test framework and follow its conventions.
- When touching untested business logic, propose backfilling tests for the
  affected area. If that is out of scope for the task, say so explicitly.
- When working in areas with poor or no test coverage, flag it — even if
  fixing it is out of scope.
- Keep business logic out of controllers, handlers, and other entry
  points. Extract it into service objects, plain classes, or modules.
- Use the framework's built-in protections for mass assignment,
  CSRF, and input filtering. Never bypass them.
- Scope data access through the current user or equivalent context
  rather than querying top-level models directly — this reduces the
  risk of exposing data the current user should not access.
- Use idiomatic null/nil handling for the language. Prefer explicit
  checks over implicit truthiness where the distinction matters.

<!-- Keep the sections relevant to your stack. Delete the rest. -->

### Ruby

- Follow the [Ruby Style Guide](https://rubystyle.guide/) unless the
  project deviates intentionally.
- Use `frozen_string_literal: true` in all Ruby files.
- Prefer `Hash#fetch` over `Hash#[]` when a missing key should raise.
- Prefer `present?` / `blank?` over nil checks where appropriate.

### TypeScript / JavaScript

- Prefer `const` over `let`. Never use `var`.
- Use strict TypeScript (`strict: true`) where the project supports it.
- Prefer `Map.get` / `Map.has` over plain object property access for
  dynamic key lookups.

### CSS

- Use a consistent naming convention for class names (e.g.
  [BEM](https://getbem.com/)).
- Keep styles logically organised — one block per file or section.
  Do not mix unrelated blocks together.

### Database

- Always write reversible migrations where possible.
- Add database-level constraints (not null, unique indexes, foreign keys)
  — do not rely solely on application-level validations.
- Never write raw SQL in application code without good reason. Use the
  project's ORM or query builder.

### Performance

- Watch for N+1 queries. Use the ORM's eager-loading mechanisms
  explicitly rather than relying on automatic or lazy loading.
- Avoid unnecessary database calls inside loops.
- Use batched iteration for large collections rather than loading
  everything into memory at once.

### Logging

- Never log tokens, passwords, API keys, or PII.
- Include enough context in log messages to be useful for debugging.

### Accessibility

- Use semantic HTML in user-facing views.
- Follow [WCAG](https://www.w3.org/WAI/standards-guidelines/wcag/) basics
  — proper heading hierarchy, alt text for images, sufficient colour
  contrast, keyboard navigability.

### Documentation

- Only add inline comments for non-obvious logic, decisions, or
  workarounds. If code needs a comment to explain what it does,
  rewrite the code.
- Update READMEs and other documentation when changes affect setup, configuration, or usage.
- Feature documentation lives in `docs/` as markdown files. Each should
  cover: the problem being solved, reasoning behind the implementation,
  and technical details of how it works. Add new documentation there
  when needed.

### Dependency Management

- Before suggesting a dependency, check that it has had a release
  or commit within the last 12 months and has no known CVEs.
- Pin dependency versions in the project's dependency manifest.
- Avoid adding unnecessary dependencies — if the standard library or
  existing dependencies can do the job, prefer those.

## Git & Workflow

- Use [Conventional Commits](https://www.conventionalcommits.org/) for all commit messages.
- Keep commits atomic — one logical change per commit.
- Write clear commit descriptions when the change is non-obvious.
- Do not amend or force-push shared branches without asking.
- Do not add co-author entries for AI tools in commit messages.

## Project Context

<!-- Adapt this section per repository -->

- **Stack:** (specify per project, e.g. Python/Django, Ruby/Rails, TypeScript/Next.js)
- **Language version:** (specify per project)
- **Framework version:** (specify per project)
- **Key dependencies:** (list per project)
- **Architecture notes:** (describe per project, e.g. monolith, API-only, microservices)
- **Deployment:** (describe per project)
