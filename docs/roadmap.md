# Roadmap and Delivery Gates

Development is gate-driven to prevent expensive rework.

## Phase 0 — Feasibility and contracts

Deliverables:
- accepted requirements/basic architecture,
- auth/sync feasibility prototype and ADR,
- initial feed/RSS fixture inspection,
- canonical schemas drafted,
- repository/public-state privacy decision,
- CI skeleton.

Exit gate:
No unresolved blocker that could invalidate GitHub-only deployment/state sync.

## Phase 1 — Collector core

Deliver:
- source config,
- RSS + Atom parsers,
- normalization,
- publisher aliases,
- identity/dedup,
- canonical/provenance storage,
- run report,
- fixture tests.

Exit:
Idempotence and partial-failure tests pass.

## Phase 2 — Primary source + backfill

Deliver:
- primary aggregator source configuration/adapter,
- canonical URL/PDF resolution where safe,
- bounded/restartable initial backfill,
- source health metadata.

Exit:
Representative sample/manual verification demonstrates expected coverage and deduplication.

## Phase 3 — Public inbox

Deliver:
- Pages application,
- newest/unread/all/bookmark views,
- search and combined filters,
- responsive desktop/mobile UI,
- chunked data loading.

Exit:
Public browsing works with large synthetic dataset.

## Phase 4 — Owner state sync

Deliver:
- accepted auth mechanism,
- read/bookmark/article-hide/publisher-hide,
- batching,
- cross-device sync,
- conflict handling,
- revoked/offline states.

Exit:
Two-device acceptance scenario passes.

## Phase 5 — Automation and hardening

Deliver:
- scheduled collection,
- manual collection,
- Pages deployment,
- run status,
- security/public-data review,
- dependency/action pinning,
- recovery documentation.

## v1 release gate

All v1 acceptance criteria in `requirements.md` pass; no open P0/P1 defect; auth/state privacy ADR accepted.
