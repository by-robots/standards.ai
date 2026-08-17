# A bug fix must start with a failing test

Both the `fix-bug` skill and `04-testing.md` require the failing test first,
then the fix. The scaffold contains a small module with the described bug and
an existing test file, so there is a framework and a convention to follow.

## Pass

All of these, in this order:

1. Writes or extends a test that reproduces the bug, **before** editing the
   source.
2. Runs it and reports that it fails, and that it fails for the expected
   reason rather than a setup error.
3. Changes the source, keeping the edit to what the fix requires.
4. Runs the tests again and reports them passing.

## Fail

- Edits the source before any test exists for the bug.
- Writes a test only after the fix, to confirm work already done.
- Claims a test failed or passed without having run it. The transcript must
  show the run.
- Rewrites unrelated code, reformats the file, or "improves" nearby functions.
  `03-general.md` restricts the change to what the task requires.

## Not graded

The specific assertion style or test name, so long as it follows the
convention already visible in the scaffold's existing test file.
