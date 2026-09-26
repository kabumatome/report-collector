# Documentation index

This directory is the canonical source of product and architecture truth.

## Core documents

| Document | Purpose |
|---|---|
| [requirements.md](requirements.md) | Functional/non-functional requirements and acceptance criteria |
| [architecture.md](architecture.md) | Basic system design and component boundaries |
| [data-model.md](data-model.md) | Canonical entities, identifiers, schemas, and invariants |
| [ingestion.md](ingestion.md) | Source strategy, normalization, deduplication, backfill, failure handling |
| [auth-and-sync.md](auth-and-sync.md) | GitHub-only authentication/state synchronization design and feasibility gate |
| [security-and-privacy.md](security-and-privacy.md) | Public-repository safety, secrets, data exposure rules |
| [testing.md](testing.md) | Test pyramid, fixtures, contracts, acceptance tests |
| [operations.md](operations.md) | Scheduling, freshness, recovery, and GitHub Actions operational risks |
| [roadmap.md](roadmap.md) | Delivery phases, gates, and definition of v1 |
| [adr/README.md](adr/README.md) | Architecture Decision Records |

## Documentation rules

- Avoid duplicate specifications. Link to the canonical section instead.
- Stable requirements receive IDs (`FR-xxx`, `NFR-xxx`, `SEC-xxx`).
- Decisions that constrain future implementation belong in ADRs.
- Open questions must have an owner/issue and a closure criterion; do not leave ambiguous TODO prose scattered through docs.
- Generated artifacts and historical notes do not belong in this directory unless they remain useful to implementation.
