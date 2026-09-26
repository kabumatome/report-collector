# Ingestion Design

## 1. Strategy

Primary strategy: low-maintenance standards-based feeds.

Priority:
1. aggregator RSS/Atom,
2. official RSS/Atom to fill coverage gaps,
3. later approved lightweight HTML discovery,
4. browser automation only as an exceptional future adapter.

## 2. Pipeline

```text
fetch -> validate response -> parse -> map raw item -> normalize publisher/URLs/time
      -> identify candidate article -> deduplicate -> persist discovery/article
      -> build run report
```

Each stage should be testable independently.

## 3. HTTP behavior

- Identify the application with a stable User-Agent.
- Reasonable connect/read timeouts.
- Bounded retries with backoff for transient failures.
- Respect HTTP cache validators (ETag/Last-Modified) where available.
- Limit redirect depth.
- Never send repository credentials to source hosts.
- Reject non-http(s) URLs from untrusted feed content.

## 4. Parser behavior

RSS/Atom parsers must tolerate common optional-field omissions.

Raw date strings are preserved in Discovery if parsing fails. A bad date must not discard the whole item.

HTML embedded in descriptions must be sanitized before frontend rendering; plain-text extraction is preferable for v1.

## 5. Publisher normalization

Order:
1. explicit source field mapping,
2. configured alias table,
3. deterministic text normalization,
4. create/retain unresolved publisher label.

No fuzzy/AI matching in v1.

## 6. Canonical/original URL resolution

For aggregator feeds, resolution may:
- follow safe HTTP redirects,
- inspect a known aggregator page adapter only when documented/tested,
- retain the aggregator URL regardless.

Resolution failure must fall back to source URL; it must not drop the item.

## 7. Deduplication

Strong matches:
- canonical URL exact after conservative normalization,
- document URL exact.

Composite match:
- publisher ID + normalized title + publication date.

If publication date is absent, composite matching is weaker and should not merge unless another strong signal exists.

## 8. Backfill

Backfill is explicitly not a scheduled crawl.

Requirements:
- bounded page/range options,
- checkpoint file/state,
- restartability,
- rate limiting,
- item-level error recording,
- same normalization/dedup path as live ingestion.

## 9. Fixture-first source development

Every new source/protocol edge case must include immutable test fixtures with:
- representative normal item,
- missing optional fields,
- duplicate/repeated item,
- malformed date where relevant,
- URL redirect/canonical behavior where relevant.

Do not make routine CI depend on live external websites.

## 10. Source health

Track:
- last successful fetch,
- last HTTP status,
- last parsed item count,
- consecutive failures,
- last error code/category.

UI may expose a concise last-update status; operational detail remains in run reports/Actions.
