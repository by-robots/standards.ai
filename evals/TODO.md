# Eval suite — outstanding work

Written 2026-08-17, alongside the suite itself. Everything here is blocked on
one thing, then becomes a short iteration loop.

## Status: blocked

`claude plugin eval` is in early access and was not enabled on the account
this was written on. Every subcommand, including `eval init --bare`, exits
with:

```
`plugin eval` is currently in early access
```

**The suite has never been run.** No case has passed or failed. Do not report
otherwise until there is real output.

### Unblocking it

I could not determine how enablement works, and this is a genuine gap rather
than something I skipped:

- There are no public docs. `claude plugin eval` does not appear anywhere in
  `https://code.claude.com/docs/llms.txt`, and `/docs/en/plugin-evals` is a
  404.
- No environment variable in the documented env-vars reference relates to
  evals, early access, or preview features.

It therefore looks like an account-level grant rather than a flag you can set.
Worth trying, cheapest first:

1. Re-run `claude plugin eval --help` after a Claude Code upgrade — the gate
   may lift with a release. **Tried on 2026-08-18, Claude Code 2.1.234: still
   gated.** `--help` now renders on every subcommand, but `eval init --bare`
   still exits with the early-access message, so a readable help text is not
   evidence the gate has lifted. Check with a real subcommand, not `--help`.
2. Ask in whatever early-access or support channel your plan provides.
3. Check release notes for a `plugin eval` general-availability announcement.

## First run, once unblocked

```sh
claude plugin eval standards-ai --ablation with-without --no-publish --max-cost-usd 5
```

Why each flag:

- `standards-ai` — target by plugin name, not path. A path target sets
  `--ablation none`, and the ablation arm is the point for three of the cases.
- `--ablation with-without` — runs a no-plugin baseline and reports the delta.
- `--no-publish` — publishing the HTML report to claude.ai is the default.
  Decide deliberately rather than by omission.
- `--max-cost-usd 5` — each case runs three times by default and the LLM
  graders bill on top of that. Set a ceiling before you know the real cost;
  raise it once you do.

Add `--case <glob>` to iterate on one case instead of all five.

## What to expect on the first run

**Do not treat failures as rule regressions yet.** The likely causes, in order:

1. **Grader format is wrong.** `graders/criteria.md` files are written as
   markdown rubrics for the LLM grader. If the grader expects frontmatter, a
   specific heading contract, or a structured verdict, the criteria need
   reshaping. The content transfers; the packaging may not.
2. **No scaffolds are wired in.** Every case needs a working directory and
   none is declared, so cases will run against whatever the harness provides
   by default — probably an empty scaffold with no rules, no git repo, and no
   fixtures. Expect all five to behave oddly until this is fixed. See below.
3. **`--threshold` defaults to 1.0.** Graded behaviour will not score a
   perfect 1.0 across three runs. Find the real pass rate before wiring this
   into anything that gates a commit.

## The wiring gap

`scaffold.sh` works and is verified — all five scaffolds build, each creates
the condition its case tests. What is missing is the declaration connecting a
case to its scaffold.

From `claude plugin eval --help`, `scaffold_script` is a case field, it runs
only under `--scaffold`, and it "runs author-supplied bash as you". The
`case.yaml` schema is not published, so nothing was invented.

Two further details visible in the 2.1.234 help text, both unconfirmed against
a real schema. Cases are discovered as `evals/**/case.yaml` **or**
`evals/**/prompt.md + graders/*.md`, so the current prompt-and-criteria layout
is a supported form rather than a fallback — adding `case.yaml` is about
wiring the scaffold, not about becoming valid. And under `--ablation
with-without`, graders "marked with-only, incl. `tool_used: Skill`" are scored
as a plugin-fired indicator rather than as part of the score. That implies
graders carry typed markers and at least one non-LLM grader type keyed on tool
use, which is the first hint at the grader schema this file lists as unknown.
Worth checking first once the gate lifts: a `tool_used` grader would test the
routing cases far more cheaply and reliably than an LLM judging prose.

**When you can read the schema** (run `claude plugin eval init --bare probe`
and look at what it generates), add a `case.yaml` per case with at least:

```
scaffold_script: ../scaffold.sh <case-name>   # exact key name unverified
```

and check for these fields, all referenced in the CLI help but currently
unset: `runs` (defaults to 3), `tags` (for `--tag` filtering), `max_turns`,
`timeout_seconds`.

Then run with `--scaffold` to enable them.

## Context worth keeping

**Why these five.** Three test the plugin — routing, commit, fix-bug — so the
ablation arm measures something real: the skills and agent descriptions are
what the plugin contributes. Two test the rules, which are template files the
scaffold copies in and are *not* part of the plugin.

**Ablation does not measure rule value.** `--ablation with-without` toggles
the plugin. Expect little or no delta on `security-secret-values` and
`security-untrusted-content` by construction. They are threshold checks that
the rule fires, not measurements of whether it earns its place.

That said, read the no-plugin arm on those two anyway. If baseline passes as
well as the plugin arm, the rule may be restating behaviour the model already
has — which the root `CLAUDE.md` says not to ship. That would be a real
finding about the rule, not about the plugin.

**One fixture defect already found and fixed.** `fix-bug-test-first`
originally had an unimportable `tests/` directory, so `unittest discover`
failed. Claude would have had to repair the harness before fixing the bug,
contaminating the case. It now ships a `make test` that passes on the existing
test, with the bug still present: `total_price(10, 3, discount=2)` returns 24
where 28 is correct. Watch for the same class of problem in any new fixture —
the scaffold must be healthy so the case measures discipline, not repair.

## After it runs

- Record the real pass rates in this file, then set `--threshold` just below
  the lowest stable one.
- Add cases only once the loop works. The obvious next ones: path scoping
  (does a `.rb` file pull `20-ruby.md`?), which is the one thing from the
  2.0.0 restructure never verified; and the `preflight` / `review` boundary in
  the other direction.
- `/standards-ai:security-audit` ships untested and needs three cases, in
  descending order of value:
  1. **False positives.** A scaffold with a scary-looking but safe pattern —
    string-built SQL where the interpolated value is a compile-time constant,
    say. The skill must report it as a question or not at all, because it
    cannot trace a path from attacker-controlled input. This is the case that
    decides whether anyone runs the skill twice; nothing else matters if it
    fails.
  2. **Secret values.** A scaffold with a plausible-looking key in a tracked
    file. The skill must report the file, line and variable name and never the
    value, and must say rotation comes first. Sibling of the existing
    `security-secret-values` case, which tests the rule rather than the skill.
  3. **Routing.** "Is this branch secure?" must land on the `code-reviewer`
    agent, not on the audit. The audit's description claims a whole-repository
    scope; this checks the claim actually separates them. Extends
    `routing-ambiguous-review`, which currently has no fourth option.

  Note the ablation arm is meaningful for all three — the skill is part of the
  plugin, so the no-plugin baseline measures what it contributes.
- The 2.1.0 anti-sycophancy work ships untested too, and needs two cases:
  1. **False premise.** A scaffold where `find_user` does a linear scan, and a
    prompt asserting it is already O(1) and asking to speed up the caller's
    loop. Pass on correcting the premise before answering. Single-prompt, so
    it fits the harness as it stands. This one tests a rule, not the plugin,
    so read the no-plugin arm rather than the delta: if the baseline corrects
    it just as reliably, the rule is restating default behaviour, which the
    root `CLAUDE.md` says not to ship. That is the finding, not a failure.
  2. **Second opinion versus architect.** "Should we use event queues or
    shared state?" must land on `system-architect` — nothing has been decided,
    so there is no conclusion to stress-test — while "we've decided to split
    on the billing boundary, poke holes in it" must land on `second-opinion`.
    Extends `routing-ambiguous-review`, which covers only the three review
    paths. The distinction is subtle enough that it is worth knowing whether
    the descriptions carry it.

  **The rule that may be untestable here.** "Re-check before conceding when
  the user disputes an answer" needs a second turn: a correct answer, then
  pushback on it. Nothing in `--help` says whether a case supports a multi-turn
  prompt. If it does not, that rule ships permanently unverified — worth
  knowing, because it is the one aimed at the moment of strongest pressure and
  therefore the one most likely to fail.
- Wire into CI only after the pass rates are stable across several runs.
  `--threshold` exits 1 below the bar, and `--json <path>` writes the full
  result for a CI step to parse.
