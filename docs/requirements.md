# Product Requirements

Status: Draft for design review  
Product: report-collector

## 1. Product goal

Provide a low-maintenance, GitHub-hosted inbox for discovering and organizing newly published Japanese economic, policy, research, and institutional reports.

The primary job is **daily information collection**, not AI analysis or statistical-data warehousing.

## 2. Scope

### In scope for v1

- Scheduled collection from RSS/Atom feeds.
- Economic/report aggregator feed as the initial primary source.
- Optional official feeds as supplemental sources.
- Normalized publisher/source metadata.
- Deduplication across multiple discovery sources.
- Newest-first inbox.
- Search/filter by publisher, tags, date, PDF presence, read state, bookmark state.
- Automatic read marking when an article/PDF link is opened; reversible.
- Star bookmark; bookmark-only view.
- Per-article hide/unhide.
- Per-publisher hide/unhide that also affects future articles.
- Public read-only browsing.
- Authenticated owner-only state mutation.
- Cross-device state synchronization using GitHub as the only persistence/service platform.
- GitHub Actions scheduled collection and manual execution.
- GitHub Pages deployment.
- Initial historical backfill where technically/reasonably available.
- Mobile-first responsive UI.

### Explicitly out of scope for v1

- AI classification, summarization, embeddings, RAG.
- External databases/services (Supabase, Firebase, Cloudflare, etc.).
- Multi-user social features.
- Statistical-series database.
- General-purpose crawler framework.
- Mandatory article-body archiving.
- Mandatory PDF archiving.
- Source-specific browser automation unless a later approved requirement demands it.

## 3. Personas and access

### P1 Owner
Single primary user. Can browse and mutate personal state.

### P2 Public viewer
May browse published report metadata. Cannot mutate owner state.

## 4. Functional requirements

### Collection

**FR-001** The system shall support multiple enabled/disabled sources configured without modifying core collection logic when the source uses an already-supported protocol.

**FR-002** v1 shall support RSS 2.0 and Atom.

**FR-003** Each collection run shall be idempotent: reprocessing the same feed items must not create duplicate logical articles.

**FR-004** A failure in one source shall not block successful processing of other sources.

**FR-005** Existing stored articles shall never be removed solely because they disappear from a feed.

**FR-006** Each discovery event shall retain its source provenance.

**FR-007** If an aggregator item can be resolved to an original publisher URL, the system shall store both aggregator/source URL and canonical publisher URL.

**FR-008** If a direct PDF/document URL is discoverable, it shall be stored separately from the article URL.

**FR-009** Source-provided tags/categories/keywords may be retained verbatim. The system shall not invent semantic tags in v1.

**FR-010** The default schedule shall run twice daily, with manual execution available.

**FR-011** Each run shall emit machine-readable run status including start/end, per-source outcome, discovered count, inserted count, deduplicated count, and error summaries.

### Historical import

**FR-020** Backfill shall be a separate command/path from normal scheduled feed ingestion.

**FR-021** Backfill must be restartable and idempotent.

**FR-022** Backfill shall stop or degrade gracefully when an upstream source blocks, throttles, or changes format.

### Inbox

**FR-030** Default view shall show non-hidden unread articles ordered by publication time descending; discovery time is the fallback if publication time is unavailable.

**FR-031** The UI shall provide views for unread, all, and bookmarked items.

**FR-032** Article cards shall show at least title, publisher, publication time/date, source tags when available, and article/PDF actions when available.

**FR-033** Opening the article or PDF action shall mark the item read before navigation; the user shall be able to revert it to unread.

**FR-034** Bookmark shall be a boolean star state and shall be independently combinable with read/hidden state.

**FR-035** Hiding an article shall remove it from normal views/search while retaining it in storage.

**FR-036** A dedicated hidden-items view shall allow restoration.

**FR-037** Hiding a publisher shall hide all current and future articles from that publisher from normal views without stopping ingestion.

**FR-038** A settings view shall list hidden publishers and allow restoration.

**FR-039** Search shall cover title, normalized publisher name, description/summary when provided by source, and source tags.

**FR-040** Filters shall be combinable, including publisher, tag, date range, PDF presence, read state, and bookmark state.

**FR-041** Pagination/virtualization shall prevent unbounded DOM rendering as the archive grows.

### State synchronization

**FR-050** Article state shall include `read`, `bookmarked`, and `hidden` as independent flags.

**FR-051** Publisher state shall include `hidden`.

**FR-052** The canonical user state shall live in GitHub, not only in browser storage.

**FR-053** PC and smartphone shall converge on the same canonical user state after synchronization.

**FR-054** Public viewers shall not be able to mutate owner state.

**FR-055** Failed state writes shall be visible to the owner and retryable; the UI must not silently drop changes.

**FR-056** State writes shall use batching/debouncing to avoid a commit for every single click.

**FR-057** Concurrent edits shall use optimistic concurrency and deterministic merge behavior.

### Public/private behavior

**FR-060** Report metadata may be publicly visible.

**FR-061** No secret, credential, token, email address, local filesystem path, or confidential content shall be published as application data.

**FR-062** Owner behavioral state is considered potentially personal metadata. Its repository/publication location must be explicitly chosen by the accepted authentication/sync ADR before production deployment.

## 5. Non-functional requirements

**NFR-001 Reliability** Collection must be idempotent and safe to retry.

**NFR-002 Maintainability** Adding a standard RSS/Atom source should require configuration plus tests, not a new scraper.

**NFR-003 Cost** No paid runtime infrastructure is required. GitHub Actions usage should remain modest.

**NFR-004 Performance** Initial inbox should become interactive promptly on ordinary mobile connections; data must be chunkable/paginated rather than one indefinitely growing monolith.

**NFR-005 Portability** Core ingestion logic shall run locally and in GitHub Actions.

**NFR-006 Observability** Collection/deployment failures shall be diagnosable from Action logs and run metadata without reproducing locally first.

**NFR-007 Accessibility** Keyboard operation, semantic controls, readable focus state, and basic WCAG-oriented contrast are required.

**NFR-008 Time** Persist timestamps in UTC ISO-8601; display in Asia/Tokyo by default.

**NFR-009 Determinism** Given identical source fixtures and prior data, normalization/deduplication results shall be deterministic.

## 6. Data retention

- Collected metadata: retained indefinitely unless a later explicit retention policy is adopted.
- Removed upstream items: retained.
- User-hidden items: retained.
- Runtime logs: governed by GitHub Actions retention; no application dependency on indefinite logs.
- Credentials: never persisted in repository data.

## 7. Acceptance criteria for v1

v1 is not complete until:
1. Two independently configured RSS/Atom sources can be ingested.
2. The same logical article discovered by both can be represented once in the inbox with provenance preserved.
3. A scheduled Action can run twice daily and a manual run can be triggered.
4. A failed feed does not corrupt existing data or block successful feeds.
5. Public Pages can browse/search/filter collected metadata on desktop and mobile.
6. Owner can change read/bookmark/article-hide/publisher-hide state and observe it on a second device.
7. Public viewer cannot alter owner state.
8. Authentication/sync passes the dedicated feasibility/security gate in `docs/auth-and-sync.md`.
9. Secret-scanning/public-repo review finds no credentials or private data.
10. Automated tests cover ingestion normalization, deduplication, state merge, and main UI state transitions.
