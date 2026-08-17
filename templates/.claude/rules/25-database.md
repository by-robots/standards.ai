---
paths:
  - "**/migrations/**"
  - "**/migrate/**"
  - "**/*.sql"
  - "**/schema.{rb,sql,prisma}"
  - "**/models/**"
  - "**/repositories/**"
  - "**/queries/**"
---

<!-- These paths cover two things: where migrations live, and where queries are
     written. The N+1 and raw-SQL rules matter in application code, not just in
     migration files, so keep the query-side paths pointed at this project's
     actual layout. Delete the file outright if the project has no database. -->

# Database

- Write reversible migrations. If a migration cannot be reversed, add a comment in the file explaining why.
- Add database-level constraints (not null, unique indexes, foreign keys) — do not rely solely on application-level validations.
- Do not write raw SQL in application code. Use the project's ORM or query builder. If raw SQL is unavoidable, document why in a comment.
- Identify N+1 queries and resolve them using the ORM's eager-loading mechanisms. Do not rely on automatic or lazy loading.
- Avoid unnecessary database calls inside loops.
