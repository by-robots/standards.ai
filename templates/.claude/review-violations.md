# Accepted Violations

Violations listed here are suppressed in `/review`. Entries may become stale as the
codebase evolves — review them periodically.

Each entry must include the file path, a context identifier, and the rule being
suppressed. The context should be the most specific stable identifier available:
a method or class name for application code, or a table/column name for migrations.
A reason is optional but recommended.

## Entries

<!-- Example:
- File: `db/migrate/20240101000000_create_users.rb`
  Context: `users.email`
  Rule: Database — Add database-level constraints
  Reason: constraint added in 20240312_add_not_null_to_users
-->
