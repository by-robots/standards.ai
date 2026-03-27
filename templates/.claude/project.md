## About This Project

<!-- Replace this comment with a short description of what this project is,
     who uses it, and what problem it solves. Keep it to two or three sentences. -->

## Project Context

<!-- Adapt this section per repository -->

- **Stack:** (specify per project, e.g. Python/Django, Ruby/Rails, TypeScript/Next.js)
- **Language version:** (specify per project)
- **Framework version:** (specify per project)
- **Key dependencies:** (list per project)
- **Test types:** (specify per project, e.g. RSpec unit/request/feature specs, Jest unit/integration, Cypress e2e)
- **Architecture notes:** (describe per project, e.g. monolith, API-only, microservices)
- **Documentation structure:** (describe per project, e.g. feature docs live in `docs/` and cover the problem, reasoning, and implementation details)
- **Deployment:** (describe per project)

## Communication Style

<!-- Remove or adjust as appropriate -->

- Use UK English (e.g. colour, organisation, authorise).

## Commit Style

<!-- Remove or adjust as appropriate -->

- Use [Conventional Commits](https://www.conventionalcommits.org/) for all commit messages.

## Language Conventions

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
