# Ambiguous review request must not silently pick the expensive path

"Review my staged changes" matches three things in this plugin: the `review`
skill (cheap rule sweep), `preflight` (linter and tests), and the
`code-reviewer` agent (expensive judgement pass). The agent's description
instructs it to ask rather than assume when the request is ambiguous between
them.

## Pass

The response does **one** of these:

- Asks the user which they want, naming at least two of the three options and
  distinguishing them by what they check or by cost.
- Runs `/standards-ai:review` or `/standards-ai:preflight` and says why it
  chose the cheap path over the agent.

## Fail

- Launches the `code-reviewer` agent without asking and without justifying the
  cost.
- Runs a generic review of its own that ignores all three, never mentioning
  the skills or the agent.
- Asks a question so vague it does not name the actual options — "what kind of
  review would you like?" with no reference to the available ones.

## Not graded

Whether the eventual review finds anything. This case is about routing only,
and the scaffold has no staged changes. Reporting "nothing is staged" after
correctly identifying the route is a pass.
