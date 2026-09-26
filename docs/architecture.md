# Basic Architecture

## 1. Design principles

1. GitHub-first: repository + Actions + Pages + GitHub API.
2. Static frontend; no hidden server assumption.
3. Standards before scraping.
4. Raw discovery provenance and normalized article identity are separate concepts.
5. User state is separate from collected report data.
6. Every write path is retryable and conflict-aware.
7. Generated/public data contracts are versioned.

## 2. Logical components

```text
RSS/Atom sources
      |
      v
[Fetcher] -> [Parser] -> [Normalizer] -> [Identity/Dedup] -> [Repository Store]
                                                           |
                                                           +-> public data build
                                                           |
GitHub Actions --------------------------------------------+
                                                           v
                                                     GitHub Pages
                                                           |
                                        +------------------+------------------+
                                        |                                     |
                                  public viewer                         authenticated owner
                                                                              |
                                                                              v
                                                                     [State Sync Adapter]
                                                                              |
                                                                              v
                                                                          GitHub API
```

## 3. Runtime boundaries

### Collector
CLI application invoked locally or by GitHub Actions.

Responsibilities:
- fetch configured feeds,
- parse protocol,
- normalize records,
- resolve safe redirects/links when configured,
- deduplicate,
- append/update provenance,
- write deterministic output,
- emit run report.

Collector must not contain UI state logic.

### Static data builder
Transforms canonical storage into frontend-consumable versioned JSON chunks/indexes.

Responsibilities:
- generate bounded-size files,
- precompute publisher/tag indexes where beneficial,
- exclude non-public state/secrets,
- validate schema before publish.

### Web application
Static SPA/PWA hosted on Pages.

Responsibilities:
- list/filter/search,
- local optimistic UI state,
- authentication/connect flow,
- synchronization through the selected GitHub-only adapter.

It must never contain embedded credentials.

### State sync adapter
Frontend abstraction. The exact authentication mechanism is gated by ADR/feasibility testing.

Contract:
- `authenticate()`
- `loadState()`
- `applyMutations(batch)`
- `logout()`
- explicit conflict/error outcomes.

## 4. Repository layout target

```text
/
├─ AGENTS.md
├─ README.md
├─ CONTRIBUTING.md
├─ SECURITY.md
├─ docs/
│  ├─ README.md
│  ├─ requirements.md
│  ├─ architecture.md
│  ├─ data-model.md
│  ├─ ingestion.md
│  ├─ auth-and-sync.md
│  ├─ security-and-privacy.md
│  ├─ testing.md
│  ├─ roadmap.md
│  └─ adr/
├─ config/
│  ├─ sources.yaml
│  └─ publisher-aliases.yaml
├─ schemas/
│  ├─ article.schema.json
│  ├─ source.schema.json
│  ├─ user-state.schema.json
│  └─ run-report.schema.json
├─ collector/
├─ web/
├─ tests/
│  └─ fixtures/
├─ data/
│  ├─ canonical/
│  ├─ provenance/
│  └─ runs/
└─ .github/
   ├─ ISSUE_TEMPLATE/
   ├─ pull_request_template.md
   └─ workflows/
```

Generated public-site output should not be mixed into source directories.

## 5. Storage choice

v1 shall prefer repository-tracked structured files over introducing a database service.

However, storage format must not assume a single ever-growing JSON object. Use sharding/chunking by stable dimensions (for example year/month or hash prefix) and generated indexes.

Rationale:
- mergeability,
- bounded diffs,
- fast client fetch,
- manageable Git history,
- easier future migration.

## 6. Identity and deduplication

Logical Article identity is distinct from Discovery identity.

Priority signals:
1. normalized canonical URL,
2. normalized document URL,
3. deterministic composite fingerprint: publisher + normalized title + publication date.

Weak heuristics must not silently merge records. Ambiguous matches should remain separate and be diagnosable.

## 7. Update semantics

A rediscovered article may enrich metadata without changing its stable article ID.

Rules:
- provenance is append/update-safe,
- non-empty trusted canonical fields may replace weaker fallback fields under explicit precedence rules,
- source disappearance never deletes,
- source errors never truncate output,
- writes are atomic from the consumer perspective.

## 8. Deployment

Separate workflows:
- CI: pull requests / pushes; tests and schema validation.
- Collect: scheduled + manual; writes canonical data/run reports.
- Pages: build/deploy from validated default-branch state.

Avoid combining all concerns into one workflow because failure and permission scopes differ.

## 9. Versioning

Each published data contract shall contain a schema version.

Breaking schema changes require:
- schema version bump,
- migration path or rebuild strategy,
- matching web release.

## 10. Architecture gates

Implementation of owner state mutation must not begin until the authentication/sync feasibility gate is closed. See `auth-and-sync.md`.

Backfill beyond a small fixture/sample must not begin until robots/terms/rate-limit behavior and resume semantics are reviewed.
