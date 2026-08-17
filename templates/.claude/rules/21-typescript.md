---
paths:
  - "**/*.{ts,tsx,mts,cts}"
  - "**/*.{js,jsx,mjs,cjs}"
---

<!-- Adjust the paths above if this project keeps JavaScript somewhere unusual. -->

# TypeScript / JavaScript

- Prefer `const` over `let`. Never use `var`.
- Do not disable `strict: true` in TypeScript projects where it is already configured.
- Prefer `Map.get` / `Map.has` over plain object property access for dynamic key lookups.
