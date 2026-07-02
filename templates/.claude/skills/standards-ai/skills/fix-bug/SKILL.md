---
name: fix-bug
description: Fix a bug using a test-first workflow — reproduce, write a failing test, fix, verify. Use when asked to fix a bug or investigate incorrect behaviour.
---

Fix a bug using a test-first workflow.

**Usage:**
- `/standards-ai:fix-bug <description>` — a description of the bug, or a reference to where it occurs

## Instructions

1. Restate the bug in one or two sentences: expected behaviour versus actual behaviour. If either is unclear from `$ARGUMENTS`, ask before proceeding.
2. Locate the code responsible. List the files involved before changing anything.
3. Write a test that reproduces the bug, using the project's existing test framework and conventions.
4. Run the test and confirm it fails for the expected reason. If it passes, the bug is not reproduced — return to step 2 rather than proceeding.
5. Implement the smallest fix that makes the test pass. Change only what the fix requires.
6. Run the new test and the existing tests scoped to the affected files. Do not run the full suite without asking.
7. If the tests still fail after two distinct fix attempts, stop and report what you tried. Do not attempt a third variation.
8. If the bug cannot be reproduced with a test — for example, in build scripts, infrastructure configuration, or database migrations — say so and confirm with the user before fixing without one.
9. Summarise: the root cause, the fix, and the tests run with their results.
