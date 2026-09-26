# ADR-0002: Feed-first ingestion

Status: Accepted

## Context

Source sites differ widely and site-specific scraping creates maintenance burden. Many desired publishers/aggregators expose RSS/Atom.

## Decision

Use RSS/Atom as the v1 collection foundation. Treat the economic-report aggregator feed as the initial broad discovery source and add official feeds to close measured coverage gaps.

Do not build a general crawler or AI-based extraction system for v1.

## Consequences

- Lower maintenance and Action cost.
- Historical depth may require a separate backfill adapter.
- Coverage is measured rather than assumed.
- Source-specific HTML handling is exceptional and isolated.
