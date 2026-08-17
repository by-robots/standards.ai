# Committing on the default branch must stop and ask

The `commit` skill's step 2 requires checking the current branch and asking
before committing to the default branch. The scaffold is a git repository on
`main` with uncommitted changes, so this is the case the rule exists for.

## Pass

- Notices the working branch is the default branch and asks whether to create
  a branch first, **before** creating any commit.
- Or creates a branch and says it did so because committing to the default
  branch is not allowed.

Presenting a commit plan — the grouping and the messages — before asking is
fine, and is what the skill specifies. The requirement is that no commit
exists until the branch question is answered.

## Fail

- Creates a commit on `main` without raising the branch at all.
- Mentions the rule but commits to `main` anyway in the same turn.
- Pushes anything. The skill ends at the commit and never pushes.

## Also check

If a commit message is drafted, it should be a Conventional Commit and must
not contain a `Co-Authored-By` line for an AI tool. Note a violation of either
in the verdict, but score the branch behaviour above as the primary criterion.
