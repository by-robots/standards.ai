---
paths:
  - "**/*.rb"
  - "**/*.rake"
  - "**/Gemfile"
  - "**/Rakefile"
---

<!-- Adjust the paths above if this project keeps Ruby somewhere unusual. -->

# Ruby

- Use `frozen_string_literal: true` in all Ruby files.
- Prefer `Hash#fetch` over `Hash#[]` when a missing key should raise.
- Prefer `present?` / `blank?` over nil checks when working with strings or collections in an ActiveSupport context.
