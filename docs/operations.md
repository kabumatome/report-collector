# Operations and Reliability

## 1. Scheduled collection

Default target: twice daily in Asia/Tokyo, initially around morning/evening.

GitHub Actions schedules run from the default branch. The workflow shall also expose `workflow_dispatch` for recovery/manual runs.

## 2. Scheduled-workflow inactivity risk

GitHub may automatically disable scheduled workflows in a public repository after 60 days without repository activity.

Mitigations:
- expose collection freshness prominently in the UI,
- persist last successful run timestamp in public run metadata,
- document manual re-enable/run procedure,
- treat stale last-run age as an operational fault, not "no new reports",
- consider a GitHub-native alert/check mechanism only if it does not create noisy Action usage.

The product must never silently imply freshness when collection has stopped.

## 3. Freshness states

Suggested UI state:
- healthy: last successful collection within expected window,
- delayed: beyond expected window but within tolerated grace period,
- stale/error: substantially overdue or most recent run failed.

Exact thresholds become configuration, not hard-coded business logic.

## 4. Partial failure

A collection run is `partial` when one or more sources fail but at least one succeeds.

Existing canonical data must remain available.

## 5. Recovery

Runbook must cover:
- rerun failed collection,
- manually dispatch collection,
- re-enable a disabled schedule,
- recover from invalid generated data without deleting the last known-good archive,
- roll back a bad application deployment,
- revoke/replace owner credentials without modifying public source data.

## 6. Data integrity

Collection should build candidate outputs, validate them, then publish atomically/transactionally at repository level as far as practical.

Never truncate canonical data before a fetch/parse completes successfully.

## 7. Workflow permissions

CI, collection, and Pages deployment stay separate so each can have the minimum GitHub permissions it needs.

## 8. Action-use discipline

- Normal collection should be lightweight RSS/Atom I/O.
- Live source smoke checks should not run on every PR.
- Do not use frequent scheduled runs merely as monitoring heartbeats.
- Prefer actionable logs/run metadata over repeated retry workflows.
