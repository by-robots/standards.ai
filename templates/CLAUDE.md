# CLAUDE.md

## Security — Read This First

- **Never** commit secrets, API keys, tokens, passwords, or credentials to the repository.
  Use environment variables or Rails credentials.
- **Never** run destructive operations (database drops, mass deletions, file removals) without explicit confirmation.
- **Never** disable or weaken authentication, authorisation, or encryption without explicit instruction.
- **Never** install gems or packages without asking first.
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
- When answering questions, refer back to sources (documentation, style guides, gem READMEs, etc.) where possible.
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

### Ruby / Rails

- Follow the [Ruby Style Guide](https://rubystyle.guide/) and
  [Rails Style Guide](https://rails.rubystyle.guide/) unless the project
  deviates intentionally.
- RuboCop is used for linting. Respect the project's `.rubocop.yml`
  configuration. Do not disable cops inline without good reason and
  explicit approval.
- Use RSpec for tests. Follow the [Better Specs](https://www.betterspecs.org/) conventions.
- New code should include tests for non-trivial changes.
- When touching existing untested code, always backfill tests.
- Use `frozen_string_literal: true` in all Ruby files.
- Prefer `Hash#fetch` over `Hash#[]` when a missing key should raise.
- Use service objects or POROs to keep controllers and models lean.
- Non-standard Rails objects (services, presenters, facades, etc.)
  should follow the `NameType` naming convention, e.g.
  `ProcessInputService`, `OrderPresenter`, `CreateFacade`.
- Use strong parameters. Never bypass mass assignment protection.
- Use scopes for reusable queries; keep complex queries out of
  controllers.
- Avoid querying models directly where possible, as this risks
  exposing data the current user should not access. Scope queries
  through the current user or a similar association instead.
  Bad: `Order.where(user_id: Current.user.id)`
  Good: `Current.user.orders`
- Prefer `present?` / `blank?` over nil checks where appropriate.

### JavaScript

- Follow [Standard](https://standardjs.com/) style rules.
- Prefer `const` over `let`. Never use `var`.

### SCSS

- Use [BEM](https://getbem.com/) naming convention for class names.
- Write SCSS, not plain CSS.
- Keep SCSS logically organised — one block per file or section. Do not mix unrelated blocks together.

### Database

- Always write reversible migrations where possible.
- Add database-level constraints (not null, unique indexes, foreign keys)
  — do not rely solely on model validations.
- Never write raw SQL in application code without good reason. Use ActiveRecord.

### Performance

- Watch for N+1 queries. Use `preload` or `eager_load` explicitly rather
  than `includes`, which lets Rails decide the strategy.
- Avoid unnecessary database calls inside loops.
- Use `find_each` or `find_in_batches` over `all.each` for large
  collections.

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

- Before suggesting a gem or package, check that it has had a release
  or commit within the last 12 months and has no known CVEs.
- Pin dependency versions in Gemfiles and package manifests.
- Avoid adding unnecessary dependencies — if the standard library or
  existing dependencies can do the job, prefer those.

## Git & Workflow

- Use [Conventional Commits](https://www.conventionalcommits.org/) for all commit messages.
- Keep commits atomic — one logical change per commit.
- Write clear commit descriptions when the change is non-obvious.
- Do not amend or force-push shared branches without asking.

## Project Context

<!-- Adapt this section per repository -->

- **Stack:** Ruby on Rails, PostgreSQL
- **Ruby version:** (specify per project)
- **Rails version:** (specify per project)
- **Key dependencies:** (list per project)
- **Architecture notes:** (describe per project, e.g. monolith, API-only, etc.)
- **Deployment:** (describe per project)
