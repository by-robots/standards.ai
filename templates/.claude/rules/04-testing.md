# Testing

- Before writing any code that changes business logic, state which existing tests are affected and whether new tests are needed.
- New code for business logic must include tests. Use the project's existing test framework and follow its conventions.
- When touching business logic that has no tests, propose backfilling tests. If that is out of scope, say so explicitly.
- When asked to fix a bug, first write a failing test that reproduces it, then implement the fix. If the bug cannot be reproduced with a test — for example, in build scripts, infrastructure configuration, or database migrations — confirm with the user before proceeding without one.
- When identifying what tests are needed, address all relevant layers: unit tests for isolated logic, integration tests for component interactions, and — in web applications — endpoint-level tests for HTTP routes and CRUD operations.
