---
paths:
  - "**/*.{html,htm,xhtml}"
  - "**/*.{erb,haml,slim,liquid,hbs,mustache,twig,blade.php}"
  - "**/*.{vue,svelte,astro}"
  - "**/*.{jsx,tsx}"
---

<!-- Adjust the paths above to match the template language this project uses. -->

# Mark-up

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
