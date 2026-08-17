---
name: security-audit
description: Audit the whole repository for security vulnerabilities — attack surface, injection sinks, authentication and authorisation, dependency CVEs, and committed secrets. Repository-wide, slow, and expensive; use when asked for a security audit or a periodic sweep. Not for checking a change before committing — that is the code-reviewer agent.
---

Audit the whole repository for security vulnerabilities.

**Usage:**
- `/standards-ai:security-audit` — audits the whole repository; takes no arguments

**Scope:** the repository as it stands, not a diff. If asked to audit a single
path or a branch, say this skill does not scope and offer the `code-reviewer`
agent instead. The value here is coverage; a scoped run is a different job.

## Instructions

### 1. Map the attack surface

Do this before reading any file in full. Find where untrusted data enters:
HTTP routes and their handlers, authentication and session middleware, CLI
arguments, queue and webhook consumers, deserialisation, file uploads,
template rendering, subprocess and shell invocation, and outbound requests
built from input.

Report the count per category and the entry-point list. If there are more than
40 entry points in total, present the list, propose an order, and ask whether
to audit all of them before continuing.

### 2. Run the deterministic checks

These are tool output, not judgement. Never assert a result from memory.

1. Dependency CVEs: the ecosystem's audit tool (`npm audit`, `bundle audit
   check --update`, `pip-audit`, `govulncheck`, `cargo audit`).
2. Secrets in the working tree and in history: `gitleaks detect`, or
   `trufflehog filesystem` / `git`.

For each, report the tool, the command, and its findings. If a tool is not
installed, say so and move on — do not substitute a manual scan for it and do
not present a manual scan as equivalent coverage.

### 3. Audit the source, one domain at a time

Read the entry points and the code they reach. Record findings for a domain
before starting the next.

1. **Injection sinks** — query construction by string building, shell and
   subprocess arguments, template rendering that bypasses escaping,
   deserialisation of untrusted input, file paths built from input, outbound
   URLs built from input.
2. **Authorisation** — for every entry point, identify the check that ties the
   requested record to the caller. A route that loads a record by an ID taken
   from the request without an ownership or permission check is a finding.
   This is usually the highest-yield domain; do not skip it because the code
   looks conventional.
3. **Authentication** — token and session issuance, verification, expiry and
   revocation, password storage, reset and recovery flows.
4. **Input handling** — validation at the boundary, mass assignment, upload
   type and size limits, redirect targets.
5. **Data exposure** — stack traces and internal paths in responses,
   serialisers returning more fields than the caller needs, secrets or PII in
   logs.
6. **Configuration** — debug flags enabled outside development, permissive
   CORS, cookie flags, disabled framework protections.

### 4. Qualify every finding

- Trace it. Name the entry point, the path to the sink, and quote both ends.
  A pattern match with no path from attacker-controlled input is not a
  finding — list it separately as a question.
- Cite the rule it breaks, if one does. Most of these are already covered by
  `.claude/rules/00-security.md` and `05-logging.md`.
- Rate it by who can reach it: **Critical** — an unauthenticated caller, or a
  live credential found in the repository; **High** — an authenticated user
  acting outside their own permissions; **Medium** — needs unusual
  preconditions, or the control is defence-in-depth only.
- Read `.claude/security-exceptions.md` if it exists and drop findings that
  match an entry by file path, context, and finding. Report any entry whose
  expiry date has passed as a finding again, noting that it lapsed.

### 5. Handle secrets as incidents

Report the file, the line, and the variable or key name. **Never print the
value**, in the summary or the report file. If a live credential is in the
working tree or in history, say plainly that rotation comes first — removing
it from history does not un-leak it — and do not start rewriting history.

### 6. Report

Present in the response, and keep it short:

1. One line per domain audited, with the finding count. Include domains with
   no findings, and any category from step 1 you could not assess from source,
   so coverage is visible.
2. Findings ordered by severity: one line each, `path:line`, the severity, and
   the failure in a single sentence.
3. A one-line verdict.

Do not include remediation, traces, or code quotations in the response —
those belong in the report file.

Report only what you can support from the code. If a domain is clean, say so.
Do not pad the report to look thorough.

### 7. Offer the detailed report

Ask whether to write one. Do not write it unprompted, and do not write one if
there are no findings.

Propose a path and confirm it. The detail — trace, quoted code, remediation,
tool output in full — goes there. Say once that a detailed report of unfixed
vulnerabilities is a roadmap for an attacker and should not be committed to a
public repository.

After presenting findings, if the user says a finding is accepted or not
applicable, offer to append an entry to `.claude/security-exceptions.md` with
the file path, context identifier, the finding, the reason, who accepted it,
and an expiry date.
