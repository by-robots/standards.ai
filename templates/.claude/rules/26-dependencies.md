---
paths:
  - "**/package.json"
  - "**/Gemfile"
  - "**/pyproject.toml"
  - "**/requirements*.txt"
  - "**/Cargo.toml"
  - "**/go.mod"
  - "**/composer.json"
---

<!-- Add this project's manifest if it is not listed above. -->

# Dependency Management

- Before adding a dependency, check it with the ecosystem's audit tooling (`npm audit`, `bundle audit`, `pip-audit`) where available; otherwise say the check has not been done. Never assert from memory that a dependency has no known CVEs.
- Pin dependency versions in the project's dependency manifest.
- Avoid adding unnecessary dependencies — if the standard library or existing dependencies can do the job, prefer those.
