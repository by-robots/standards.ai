Audit `.claude/review-violations.md` to remove stale entries and improve existing ones.

## Instructions

1. Read `.claude/review-violations.md`. If it does not exist or contains no entries, report that and stop.
2. For each entry:
   a. Check whether the file referenced in the entry exists.
   b. If the file exists, read it and check whether the context identifier (method, class, or table/column name) is still present.
   c. If the context is present, assess whether the rule cited in the entry is still being violated at that location.
3. Mark an entry for removal if the file no longer exists, the context identifier is no longer present, or the rule is no longer being violated at that location.
4. For entries that remain valid, check whether the context or reason field can be made more specific based on current file content. Propose improvements where they add clarity.
5. Present a summary of all proposed changes — entries to remove and entries to update — before writing anything.
6. On confirmation, update `.claude/review-violations.md` with the approved changes. Do not modify entries that were not flagged for change.
