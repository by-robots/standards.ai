# Accepted Security Findings

Findings listed here are suppressed in `/standards-ai:security-audit` until
their expiry date, after which they are reported again as lapsed.

Each entry must include the file path, a context identifier, the finding, the
reason it was accepted, who accepted it, and an expiry date. The context
should be the most specific stable identifier available: a route, method, or
class name. Keep expiry dates short — an accepted risk with a two-year expiry
is an unfixed vulnerability with paperwork.

Do not record the value of any credential here.

## Entries

<!-- Example:
- File: `app/controllers/reports_controller.rb`
  Context: `ReportsController#export`
  Finding: filename built from params without a path check
  Reason: route is behind admin auth and the parameter is enum-validated upstream
  Accepted by: <name>, 2026-01-15
  Expires: 2026-07-15
-->
