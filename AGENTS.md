# AGENTS.md

This repository is public. Treat every committed byte as internet-visible.

## Working rules

1. Read `docs/README.md`, `docs/requirements.md`, and `docs/architecture.md` before changing product behavior.
2. Requirements are canonical. If implementation needs a behavior change, update requirements/design in the same PR.
3. Never commit credentials, tokens, cookies, user identifiers, private URLs, local paths, downloaded private data, or machine-specific configuration.
4. Do not add AI/LLM features unless an accepted requirement explicitly calls for them.
5. Prefer standards-based ingestion (RSS/Atom) and configuration over source-specific scraping.
6. New source adapters must implement the shared ingestion contract and include fixtures/tests.
7. Collection failures must never delete or corrupt previously collected data.
8. User-state changes must be merge-safe and conflict-aware.
9. Keep generated/runtime data separate from source code and schemas.
10. Every PR must state scope, tests, security/privacy impact, and rollback considerations.

## Documentation map

- Product requirements: `docs/requirements.md`
- Basic architecture: `docs/architecture.md`
- Data model/contracts: `docs/data-model.md`
- Ingestion: `docs/ingestion.md`
- Authentication/state sync: `docs/auth-and-sync.md`
- Security/privacy: `docs/security-and-privacy.md`
- Test strategy: `docs/testing.md`
- Roadmap/gates: `docs/roadmap.md`
- Architecture decisions: `docs/adr/`

## Definition of done

A change is not done unless:
- behavior is covered by tests,
- public-repository safety is checked,
- docs are updated when contracts change,
- failure/retry behavior is defined,
- no new secret or external-service dependency is introduced implicitly.
