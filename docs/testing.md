# Test Strategy

## 1. Goals

Tests protect against the expensive failures for this product:
- duplicate/corrupted archive,
- silent feed loss,
- broken canonical links,
- cross-device state overwrite,
- accidental public credential/state exposure,
- mobile inbox regressions.

## 2. Layers

### Unit
- RSS/Atom mapping,
- date parsing,
- URL normalization,
- title normalization,
- publisher alias mapping,
- fingerprint generation,
- state mutation/merge.

### Contract/fixture
Protocol and source fixtures run without live network.

### Integration
- ingest fixtures into temporary canonical store,
- rerun and prove idempotence,
- two sources discover same article,
- source failure preserves old data,
- state conflict/retry simulation.

### Web component/UI
- default unread view,
- mark-read-on-open,
- revert unread,
- bookmark toggle/filter,
- article hide/restore,
- publisher hide/restore,
- combined filters,
- pending/failed sync indicators.

### End-to-end
Run against built static site and mocked GitHub/source APIs where feasible.

## 3. Live smoke tests

Live upstream checks should not gate every PR.

A lightweight scheduled/manual smoke job may validate that configured feeds still return parseable data, but fixture tests remain authoritative for CI stability.

## 4. Required regression cases

- same item appears twice in one feed,
- same article arrives from aggregator and official feed,
- publication date changes/format differs,
- article has only aggregator URL,
- PDF URL exists but article URL does not,
- source returns 500/timeout,
- feed becomes empty unexpectedly,
- malformed item among valid items,
- two devices modify different flags on same article,
- two devices modify same flag at different times,
- GitHub Contents SHA conflict,
- user token expires/revoked,
- hidden publisher later publishes a new article.

## 5. Acceptance evidence

Each roadmap gate closes only with linked automated test(s) or a documented manual feasibility result when automation is not yet possible.
